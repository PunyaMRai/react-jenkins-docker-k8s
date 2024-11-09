pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-image-name')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', 'docker-credentials') {
                        docker.image('my-image-name').push()
                    }
                }
            }
        }
    }
}
