pipeline {
    agent any

    environment {
        IMAGE_NAME = "cloud-food-app"
        CONTAINER_NAME = "cloud-food-app"
        PORT = "3000"
        DOCKER = "/usr/local/bin/docker"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/madhusudhan58/cloud-food-app.git'
            }
        }

        stage('Check Docker') {
            steps {
                sh '''
                    whoami
                    $DOCKER --version
                    $DOCKER ps
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    $DOCKER build -t $IMAGE_NAME:latest .
                '''
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                    $DOCKER stop $CONTAINER_NAME || true
                    $DOCKER rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    $DOCKER run -d \
                      --name $CONTAINER_NAME \
                      -p $PORT:$PORT \
                      $IMAGE_NAME:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    $DOCKER ps
                    $DOCKER images
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed!'
        }
    }
}