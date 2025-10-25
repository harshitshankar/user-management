pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "harshitshankar1998/usermgmt-app:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/harshitshankar/user-management.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                bat 'mvn clean package -DskipTests -U'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat 'docker login -u %USER% -p %PASS%'
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
                bat 'docker push %DOCKER_IMAGE%'
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                // Clean old containers if any
                bat 'docker-compose down || echo "No old containers"'

                // Pull latest image to ensure it uses the pushed version
                bat 'docker pull %DOCKER_IMAGE%'

                // Start all services (MySQL, Zookeeper, Kafka, App)
                bat 'docker-compose up -d --build'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed!'
        }
        success {
            echo '✅ Pipeline executed successfully! Application should be live on http://localhost:8081/api/users'
        }
        failure {
            echo '❌ Pipeline failed. Check console output for details.'
        }
    }
}
