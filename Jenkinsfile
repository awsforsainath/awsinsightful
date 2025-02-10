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
                withDockerRegistry([awsforsainath: 'dckr_pat_jKrQdHkCaRh2RasVpG77md07YdM']) {
                    sh 'docker tag awsinsightful awsforsainath/awsinsightful'
                    sh 'docker push awsforsainath/awsinsightful'
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
