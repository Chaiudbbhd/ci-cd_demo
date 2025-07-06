pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'your-dockerhub-username'
    }
    stages {
        stage('Clone Code') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }
        stage('Docker Build & Push') {
            steps {
                sh "docker build -t ${DOCKERHUB_USER}/your-app-name:latest ."
                sh "docker login -u ${DOCKERHUB_USER} -p ${DOCKERHUB_PASSWORD}"
                sh "docker push ${DOCKERHUB_USER}/your-app-name:latest"
            }
        }
        stage('Deploy to K8s') {
            steps {
                sh "kubectl apply -f deployment.yaml"
                sh "kubectl apply -f service.yaml"
            }
        }
    }
    post {
        success {
            echo 'Deployment Successful'
        }
        failure {
            echo 'Deployment Failed'
        }
    }
}
