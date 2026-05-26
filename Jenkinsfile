pipeline {
    agent any

    // For agent node:
    // agent {
    //     label 'dev'
    // }

    stages {

        stage('Clone Code!!!') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Praveen48589/Jenkins-Ci-Cd.git'
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

                        docker tag frontend-app $DockerHubUser/frontend-app:latest
                        docker tag backend-app $DockerHubUser/backend-app:latest

                        docker push $DockerHubUser/frontend-app:latest
                        docker push $DockerHubUser/backend-app:latest

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy!!!') {
            steps {
                sh '''
                    docker compose down || true
                    docker compose up -d --build --force-recreate
                '''
            }
        }
    }

    post {

        success {
            emailext(
                from: 'praveentomar740@gmail.com',
                to: 'praveentomar740@gmail.com',
                subject: "Build failed for CI-CD pipeline",
                body: "Build failed for CI-CD pipeline"
            )
        }

        failure {
            emailext(
                from: 'praveentomar740@gmail.com',
                to: 'praveentomar740@gmail.com',
                subject: "Build failed for CI-CD pipeline",
                body: "Build failed for CI-CD pipeline"
            )
        }
    }
}