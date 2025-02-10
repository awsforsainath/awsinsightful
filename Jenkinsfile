pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-creds')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/awsforsainath/awsinsightful.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t awsinsightful .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
               withDockerRegistry([credentialsId: 'docker-hub-credentials', url: 'https://index.docker.io/v1/']) {
    // Docker commands (build, push, etc.)
    sh 'docker build -t awsforsainath/myimage:latest .'
    sh 'docker push awsforsainath/myimage:latest'
}

                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['your-ec2-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ec2-54-206-50-253.ap-southeast-2.compute.amazonaws.com <<EOF
                        docker pull awsforsainath/awsinsightful
                        docker stop awsinsightful || true
                        docker rm awsinsightful || true
                        docker run -d -p 80:80 --name awsinsightful awsforsainath/awsinsightful
                        EOF
                    '''
                }
            }
        }
    }
}
