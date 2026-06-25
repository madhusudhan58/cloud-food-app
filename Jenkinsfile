pipeline {

    agent any

    environment {
        IMAGE_NAME = "cloud-food-app"
        CONTAINER_NAME = "cloud-food-container"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/madhusudhan58/cloud-food-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Remove Old Container') {
            steps {
                bat '''
                docker stop %CONTAINER_NAME% || exit 0
                docker rm %CONTAINER_NAME% || exit 0
                '''
            }
        }

        stage('Run Container') {
            steps {
                bat '''
                docker run -d --name %CONTAINER_NAME% -p 8085:80 %IMAGE_NAME%
                '''
            }
        }

    }

    post {

        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Deployment Failed'
        }

    }

}