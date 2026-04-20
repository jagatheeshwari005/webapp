pipeline {
    agent any

    environment {
        IMAGE = "jagatheeshwari/webapp"
        TAG = "%BUILD_NUMBER%"
    }

    stages {

        stage('Build') {
            steps {
                bat 'docker build -t %IMAGE%:%TAG% .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

        stage('Push') {
            steps {
                bat 'docker push %IMAGE%:%TAG%'
            }
        }

        stage('Deploy') {
            steps {
                bat 'kubectl set image deployment/webapp webapp=%IMAGE%:%TAG%'
            }
        }
    }
}