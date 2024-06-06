pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Ensure 'dockerhub-id' matches your Docker Hub credentials ID in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github', url: 'https://github.com/lab-testt/my-node-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("labbtest/my-node-app:latest") // Use your Docker Hub username
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop and remove any existing container
                    sh "docker stop my-node-app || true && docker rm my-node-app || true"
                    // Run the new container
                    dockerImage.run('-d -p 3000:3000 --name my-node-app')
                }
            }
        }
    }
}
