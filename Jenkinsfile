pipeline {
    agent any

    environment {
        IMAGE_NAME = "cloud-food-app"
        CONTAINER_NAME = "cloud-food-app"
        PORT = "3009"

        // Common Docker locations on macOS
        PATH = "/opt/homebrew/bin:/usr/local/bin:/Applications/Docker.app/Contents/Resources/bin:${env.PATH}"
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
                    echo "=============================="
                    echo "Current User:"
                    whoami

                    echo "=============================="
                    echo "Current PATH:"
                    echo $PATH

                    echo "=============================="
                    echo "Docker Location:"
                    which docker || true

                    echo "=============================="
                    echo "Docker Version:"
                    docker --version || true

                    echo "=============================="
                    docker ps || true
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker run -d \
                      --name ${CONTAINER_NAME} \
                      -p ${PORT}:${PORT} \
                      ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    docker ps
                    docker images
                '''
            }
        }
    }

    post {

        success {
            echo "Deployment Successful!"
        }

        failure {
            echo "Deployment Failed!"
        }

        always {
            sh '''
                echo "===== Docker Containers ====="
                docker ps -a || true

                echo "===== Docker Images ====="
                docker images || true
            '''
        }
    }
}