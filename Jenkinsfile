pipeline {
    agent any
    environment {
        // Docker Hub credentials (you need to configure this in Jenkins)
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE = "pun12/react-jenkins-docker-k8s"
    }
    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository from GitHub..."
                // Checkout the code from GitHub (ensure 'main' is the correct branch name)
                git branch: 'main', url: 'https://github.com/PunyaMRai/react-jenkins-docker-k8s.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building Docker image..."
                script {
                    // Build the Docker image from Dockerfile
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                script {
                    // Push the built Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                        docker.image(DOCKER_IMAGE).push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying the app to Kubernetes..."
                script {
                    // Apply Kubernetes deployment and service using kubectl
                    sh "kubectl apply -f k8s-deployment.yaml"
                    sh "kubectl apply -f k8s-service.yaml"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed."
        }
        success {
            echo "The pipeline completed successfully."
        }
        failure {
            echo "The pipeline failed. Check the logs for more details."
        }
    }
}