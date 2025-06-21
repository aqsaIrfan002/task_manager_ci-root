pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'task-manager-app'
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
                sh 'ls -la'
            }
        }
        
        stage('Environment Setup') {
            steps {
                echo 'Cleaning up existing containers...'
                sh '''
                    docker-compose -f docker-compose.ci.yml down || true
                    docker system prune -f
                '''
            }
        }
        
        stage('Build Docker Images') {
            steps {
                echo 'Building Docker images...'
                sh '''
                    docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                    docker build -t ${DOCKER_IMAGE}:latest .
                '''
            }
        }
        
        stage('Deploy Application') {
            steps {
                echo 'Deploying application...'
                sh '''
                    docker-compose -f docker-compose.ci.yml up -d --build
                    sleep 30
                    docker-compose -f docker-compose.ci.yml ps
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
            echo 'Application is running at: http://your-ec2-ip:8081'
        }
        
        failure {
            echo 'Pipeline failed!'
            sh 'docker-compose -f docker-compose.ci.yml down || true'
        }
    }
}
