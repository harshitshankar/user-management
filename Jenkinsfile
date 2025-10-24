pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "harshitshankar1998/usermgmt-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/harshitshankar/user-management.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "docker login -u $USER -p $PASS"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $DOCKER_IMAGE"
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker stop usermgmt || true"
                sh "docker rm usermgmt || true"
                sh "docker run -d --name usermgmt -p 8080:8080 $DOCKER_IMAGE"
            }
        }
    }
}
