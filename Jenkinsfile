pipeline {
    agent any

    stages {

        stage('Clone Code!!!') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Praveen48589/Food-Order-Website.git'
            }
        }

        stage('Build Frontend Image!!!') {
            steps {
                sh 'docker build -t frontend-app ./frontend'
            }
        }

        stage('Build Backend Image!!!') {
            steps {
                sh 'docker build -t backend-app ./backend'
            }
        }

        stage('Testing!!!') {
            steps {
                echo 'Testing my-app'
            }
        }

        stage('Push To DockerHub!!!') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'DockerHubCreds',
                    usernameVariable: 'DockerHubUser',
                    passwordVariable: 'DockerHubPass'
                )]) {

                    sh '''
                        echo "$DockerHubPass" | docker login -u "$DockerHubUser" --password-stdin

                        docker image tag frontend-app $DockerHubUser/frontend-app:latest
                        docker image tag backend-app $DockerHubUser/backend-app:latest

                        docker push $DockerHubUser/frontend-app:latest
                        docker push $DockerHubUser/backend-app:latest
                    '''
                }
            }
        }

        stage('Deploy!!!') {
            steps {
                sh 'docker compose up -d --build'
            }
        }
    }
}