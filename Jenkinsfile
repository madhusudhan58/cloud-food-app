pipeline {
    agent any

    stages {
        stage('Test Docker Environment') {
            steps {
                sh '''
                    export PATH=/Applications/Docker.app/Contents/Resources/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

                    echo "PATH=$PATH"

                    which docker
                    which docker-credential-desktop

                    docker --version
                    docker-credential-desktop version

                    docker pull nginx:latest
                '''
            }
        }
    }
}