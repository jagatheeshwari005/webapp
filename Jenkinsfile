pipeline {
    agent any

    environment {
        IMAGE = "jagatheeshwari/webapp"
    }

    stages {

        stage('Build') {
            steps {
                bat 'docker build -t %IMAGE%:latest .'
            }
        }

        stage('Push') {
            steps {
                bat 'docker push %IMAGE%:latest'
            }
        }

        stage('Deploy') {
            steps {
                bat 'kubectl set image deployment/webapp webapp=%IMAGE%:latest'
            }
        }
    }
}