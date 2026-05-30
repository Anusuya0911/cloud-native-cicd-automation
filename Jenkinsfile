pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "anusuya0911/devops-app"
        EC2_HOST = "3.110.40.205"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-registry-credentials',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@$EC2_HOST "
                docker pull $DOCKER_IMAGE &&
                docker stop devops-app || true &&
                docker rm devops-app || true &&
                docker run -d --name devops-app -p 8080:5000 $DOCKER_IMAGE
                "
                '''
            }
        }
    }
}
