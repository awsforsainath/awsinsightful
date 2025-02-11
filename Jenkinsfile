pipeline {
    agent any
    environment {
        // Define environment variables
        IMAGE_NAME = 'your-wordpress-custom'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code using specified Git credentials
                git credentialsId: '1a76d30f-2c64-44ad-9b40-39ea6a8de35e', url: 'https://github.com/awsforsainath/awsinsightful.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    docker.build("${env.IMAGE_NAME}")
                }
            }
        }
        stage('Deploy') {
            steps {
                // Deploy using Docker Compose
                script {
                    // Stop and remove existing containers
                    sh 'docker-compose down'
                    // Start up with the new image
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
