pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-acc'
        DOCKER_IMAGE = 'duyaccel/autobuild'
    }

    stages {
        
        stage('Clone Repository') {
            steps {
                // Clone the repository containing the Dockerfile
                git clone 'https://github.com/DuyAccel/Jenkins-Docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        // Push the Docker image to Docker Hub
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image has been successfully pushed to Docker Hub!'
        }
        failure {
            echo 'The build or push process failed!'
        }
    }
}
