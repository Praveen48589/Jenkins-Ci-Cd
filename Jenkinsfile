@Library("Shared-lib") _

pipeline {
    agent any

    // For agent node:
    //    agent {
    //    label 'dev'
    //  } 

    stages {

        stage('Clone Code!!!') {
            steps {
                script{
                    clone("https://github.com/Praveen48589/Jenkins-Ci-Cd.git", "main")
                }
            }
        }

        stage('Build Frontend Image!!!') {
            steps {
                script{
                    build("frontend-app","./frontend")
                }
            }
        }

        stage('Build Backend Image!!!') {
            steps {
                script{
                    build("backend-app","./backend")
                }
                
            }
        }

        stage('Testing!!!') {
            steps {
                echo 'Testing my-app'
            }
        }

        stage('Push To DockerHub!!!') {
            steps {
                script{
                        dockerhubPush("DockerHubCreds","frontend-app","backend-app")

                }
            }
        }

        stage('Deploy!!!') {
            steps {
                script{
                    deploy()
                }

            }
        }
    }

    post {

        success {
            emailext(
                from: 'praveentomar740@gmail.com',
                to: 'praveentomar740@gmail.com',
                subject: "Build Passed for CI-CD pipeline",
                body: "Build Passed for CI-CD pipeline"
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
