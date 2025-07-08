# 🚀 DevOps Automation: CI/CD Pipeline for Java Spring Boot App


<p align="center">
  <img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*sHJOQalCY1VQeBg-CbF68w.gif" alt="Devops" width="500"/>
</p>


This project demonstrates a **fully automated CI/CD pipeline** to build, test, containerize, and deploy a **Java Spring Boot Application** using:

✅ Jenkins + Docker + Docker Hub + Kubernetes + GitHub

---

## 📌 Project Goal

Automate the entire software delivery lifecycle for a Java application to ensure:

- Continuous Integration (CI)
- Continuous Deployment (CD)
- Zero manual intervention
- Scalable deployment using Kubernetes

---

## ⚙️ Tech Stack

| Tool            | Purpose                            |
|-----------------|-------------------------------------|
| Java (Spring Boot) | Backend API (`/hello`)             |
| Maven           | Build and dependency management     |
| Docker          | Containerization                    |
| Docker Hub      | Public image repository             |
| Kubernetes      | App orchestration (Docker Desktop)  |
| Jenkins         | CI/CD Automation                    |
| GitHub          | Source code and Jenkins trigger     |

---

## 📁 Project Structure

ci-cd_demo/
├── Jenkinsfile
├── Dockerfile
├── deployment.yaml
├── service.yaml
├── src/
├── pom.xml
└── README.md


---

## 🏗️ Setup Instructions

### 🔹 Phase 1: Java App Setup


```bash
./mvnw clean package
🔹 Phase 2: Docker Setup

docker build -t chaitu2005/demoapp:latest .
docker login -u chaitu2005
docker push chaitu2005/demoapp:latest


🔹 Phase 3: Kubernetes Setup
deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: java-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: java-app
  template:
    metadata:
      labels:
        app: java-app
    spec:
      containers:
      - name: java-app
        image: chaitu2005/demoapp:latest
        ports:
        - containerPort: 8080


🔹 Phase 4:service.yaml

apiVersion: v1
kind: Service
metadata:
  name: java-app-service
spec:
  type: NodePort
  ports:
  - port: 8080
    targetPort: 8080
    nodePort: 30080
  selector:
    app: java-app
bash
Copy
Edit
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

🌐 Access Application
kubectl get nodes -o wide
Use IP from the output like: 192.168.65.3


Access app:
http://192.168.65.3:30080/hello

🧪 Jenkinsfile - Pipeline Stages
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
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKERHUB_PASSWORD')]) {
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
            echo '✅ Deployment Successful'
        }
        failure {
            echo '❌ Deployment Failed'
        }
    }
}


🧩 Pipeline Flow

GitHub Push ➔ Jenkins ➔ Maven Build ➔ Docker Image ➔ Docker Hub ➔ Kubernetes Deployment ➔ Live API


📌 GitHub Repository

🔗 GitHub: https://github.com/Chaiudbbhd/ci-cd_demo

📸 Project Architecture

🔜 Coming Soon

GitHub Webhook Integration

Slack Notifications in Jenkinsfile

Kubernetes monitoring with Prometheus + Grafana

Auto-scaling and load balancing

🙌 Let's Connect
If you're interested in DevOps or cloud deployment and want to learn together, feel free to connect with me on LinkedIn.

#DevOps #Java #SpringBoot #Docker #Kubernetes #Jenkins #CI_CD #CloudComputing #Automation

yaml
Copy
Edit
