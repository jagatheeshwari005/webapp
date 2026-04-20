pipeline {
    agent any

    environment {
        IMAGE = "your-dockerhub-username/webapp"
    }

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE:$BUILD_NUMBER .'
            }
        }

        stage('Push') {
            steps {
                sh 'docker push $IMAGE:$BUILD_NUMBER'
            }
        }

        stage('Deploy') {
            steps {
                sh 'kubectl set image deployment/webapp webapp=$IMAGE:$BUILD_NUMBER'
            }
        }
    }
}