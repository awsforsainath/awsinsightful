pipeline {
    agent any
    environment {
        IMAGE_NAME = 'your-wordpress-custom'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Checking out source code from GitHub..."
                    try {
                        git branch: 'main', credentialsId: '1a76d30f-2c64-44ad-9b40-39ea6a8de35e', url: 'https://github.com/awsforsainath/awsinsightful.git'
                    } catch (Exception e) {
                        error "Git checkout failed: ${e.message}"
                    }
                }
            }
        }
        stage('Verify Git Installation') {
            steps {
                script {
                    echo "Verifying Git installation..."
                    sh 'git --version' // Ensures Git is installed
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${env.IMAGE_NAME}"
                    try {
                        docker.build("${env.IMAGE_NAME}")
                    } catch (Exception e) {
                        error "Docker build failed: ${e.message}"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application using Docker Compose..."
                    try {
                        sh 'docker-compose down || true' // Ensures containers stop without failing
                        sh 'docker-compose up -d'
                    } catch (Exception e) {
                        error "Deployment failed: ${e.message}"
                    }
                }
            }
        }
    }
    post {
        failure {
            echo "Build failed. Please check the logs for more details."
        }
        success {
            echo "Pipeline executed successfully!"
        }
    }
}
