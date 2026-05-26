pipeline {
    agent any
    // agent {
    //     label 'dev'   >> for agent Node 
    // }

    // agent {
    //     label 'dev'   >> for agent Node 
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
<<<<<<< HEAD
                sh 'docker compose down'
                sh 'docker compose up -d --build --force-recreate'
            }
        }
    }

    post {

        success {
            script {
                emailext(
                    from: "praveentomar740@gmail.com",
                    to: "praveentomar740@gmail.com",
                    body: "Build success for CI-CD pipeline",
                    subject: "Build success for CI-CD pipeline"
                )
            }
        }

        failure {
            script {
                emailext(
                    from: "praveentomar740@gmail.com",
                    to: "praveentomar740@gmail.com",
                    body: "Build failed for CI-CD pipeline",
                    subject: "Build failed for CI-CD pipeline"
                )
=======
                
                sh 'docker compose down'
                       
                sh 'docker compose up -d --build --force-recreate'
>>>>>>> 49b500401e2b255507ddd7b3bb6742c063684b3d
            }
        }
    }
}
