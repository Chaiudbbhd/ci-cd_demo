pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'chaitu2005'
        IMAGE_NAME = 'demoapp'
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
                script {
                    def version = "${BUILD_NUMBER}"
                    def fullImageName = "${DOCKERHUB_USER}/${IMAGE_NAME}:${version}"

                    withCredentials([usernamePassword(credentialsId: 'dockerhub-token', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker build -t ${fullImageName} ."
                        sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                        sh "docker push ${fullImageName}"
                    }

                    // Save the image name for the next stage
                    env.FULL_IMAGE = fullImageName
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl set image deployment/java-app-deployment java-app-container=${env.FULL_IMAGE}"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful'
        }
        failure {
            echo '❌ Deployment Failed'
        }
    }
}
