pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-creds')  // Ensure this matches your credential ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/awsforsainath/awsinsightful.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Building Docker image
                sh 'docker build -t awsforsainath/awsinsightful .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Push image to Docker Hub with credentials
                withDockerRegistry([credentialsId: 'docker-hub-creds', url: 'https://index.docker.io/v1/']) {
                    sh 'docker push awsforsainath/awsinsightful:latest'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['your-ec2-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ec2-54-206-50-253.ap-southeast-2.compute.amazonaws.com <<EOF
                        docker pull awsforsainath/awsinsightful:latest
                        docker stop awsinsightful || true
                        docker rm awsinsightful || true
                        docker run -d -p 80:80 --name awsinsightful awsforsainath/awsinsightful:latest
                        EOF
                    '''
                }
            }
        }
    }
}
