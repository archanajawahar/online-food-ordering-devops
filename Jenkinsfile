pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'archanajawahar14/online-food-ordering'
        DOCKER_CREDENTIALS = 'dockerhub-userpass-q2-fresh'
        K8S_CREDENTIALS = 'kubernetes-config'
        K8S_DEPLOYMENT = 'online-food-ordering-blue'
        K8S_CONTAINER = 'online-food-ordering'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Files') {
            steps {
                bat '''
                    if not exist Dockerfile exit /b 1
                    if not exist index.html exit /b 1
                    if not exist deployment.yaml exit /b 1
                '''
            }
        }

        stage('Build Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:v%BUILD_NUMBER% .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-userpass-q2-fresh',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                        @echo off
                        docker login -u "%DOCKER_USER%" --password-stdin < nul
                    '''
                }
            }
        }
    }
}
