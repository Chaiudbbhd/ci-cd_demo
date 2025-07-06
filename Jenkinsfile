pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'chaitu2005'
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
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKERHUB_PASSWORD')]) {
                    sh "docker build -t ${DOCKERHUB_USER}/demoapp:latest ."
                    sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USER} --password-stdin"
                    sh "docker push ${DOCKERHUB_USER}/demoapp:latest"
                }
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
