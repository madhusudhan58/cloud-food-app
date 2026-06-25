pipeline {
    agent any

    stages {
        stage('Test Docker') {
            steps {
                sh '''
                    echo "User:"
                    whoami

                    echo "PATH:"
                    echo $PATH

                    echo "Docker location:"
                    ls -l /usr/local/bin/docker

                    echo "Docker version:"
                    /usr/local/bin/docker --version
                '''
            }
        }
    }
}