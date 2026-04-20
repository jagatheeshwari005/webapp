pipeline {
    agent any

    environment {
        IMAGE = "jagatheeshwari/webapp"
        KUBECONFIG_PATH = "C:\\jenkins\\.kube\\config"
    }

    stages {

        stage('Build') {
            steps {
                bat 'docker build -t %IMAGE%:%BUILD_NUMBER% .'
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
                bat 'docker push %IMAGE%:%BUILD_NUMBER%'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat """
                set KUBECONFIG=%KUBECONFIG_PATH%
                kubectl get nodes
                kubectl set image deployment/webapp webapp=%IMAGE%:%BUILD_NUMBER%
                """
            }
        }
    }
}