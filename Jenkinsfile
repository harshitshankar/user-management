pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "harshitshankar1998/usermgmt-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git branch: 'main', url: 'https://github.com/harshitshankar/user-management.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                // Use 'bat' for Windows
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat 'docker login -u %USER% -p %PASS%'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop container if already running
                bat 'docker stop usermgmt || echo Container not running'
                bat 'docker rm usermgmt || echo Container not existing'

                // Run container
                bat 'docker run -d --name usermgmt -p 8080:8080 %DOCKER_IMAGE%'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed!'
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check console output.'
        }
    }
}
