pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/your-username/your-repo.git', branch: 'main'
            }
        }

        stage('Build with Docker Compose') {
            steps {
                script {
                    sh 'docker-compose -p ci_task_manager -f docker-compose.yml up -d --build'
                }
            }
        }
    }
}
