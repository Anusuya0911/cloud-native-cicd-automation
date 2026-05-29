pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "anusuya0911/devops-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Anusuya0911/cloud-native-cicd-automation.gi'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'

                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
    }
}
