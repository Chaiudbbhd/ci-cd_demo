🚀 DevOps Automation: CI/CD Pipeline for Java Spring Boot App
<p align="center"> <img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*sHJOQalCY1VQeBg-CbF68w.gif" alt="DevOps Pipeline" width="500"/> </p>
This project demonstrates a fully automated CI/CD pipeline to build, test, containerize, and deploy a Java Spring Boot Application using:

✅ Jenkins + Docker + Docker Hub + Kubernetes + GitHub

📌 Project Goal
Automate the entire software delivery lifecycle for a Java application to ensure:

Continuous Integration (CI)

Continuous Deployment (CD)

Zero manual intervention

Scalable deployment using Kubernetes

⚙️ Tech Stack
Tool	Purpose
Java (Spring Boot)	Backend API (/hello)
Maven	Build and dependency management
Docker	Containerization
Docker Hub	Public image repository
Kubernetes	App orchestration (Docker Desktop)
Jenkins	CI/CD Automation
GitHub	Source code & Jenkins trigger

📁 Project Structure
bash
Copy
Edit
ci-cd_demo/
├── Jenkinsfile            # CI/CD Pipeline definition
├── Dockerfile             # Containerization config
├── deployment.yaml        # Kubernetes Deployment
├── service.yaml           # Kubernetes Service
├── src/                   # Java Spring Boot source code
├── pom.xml                # Maven configuration
└── README.md
🏗️ Setup Instructions
🔹 Phase 1: Java App Setup
bash
Copy
Edit
./mvnw clean package
🔹 Phase 2: Docker Setup
bash
Copy
Edit
docker build -t chaitu2005/demoapp:latest .
docker login -u chaitu2005
docker push chaitu2005/demoapp:latest
🔹 Phase 3: Kubernetes Setup
deployment.yaml:

yaml
Copy
Edit
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
service.yaml:

yaml
Copy
Edit
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
Apply the configurations:

bash
Copy
Edit
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
🌐 Access Application
Get your Kubernetes node IP:

bash
Copy
Edit
kubectl get nodes -o wide
Access the app in your browser:

arduino
Copy
Edit
http://<Node-IP>:30080/hello
Example:
http://192.168.65.3:30080/hello

🧪 Jenkinsfile - Pipeline Stages
groovy
Copy
Edit
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
mathematica
Copy
Edit
GitHub Push ➔ Jenkins ➔ Maven Build ➔ Docker Image ➔ Docker Hub ➔ Kubernetes Deployment ➔ Live API
📌 GitHub Repository
🔗 GitHub: https://github.com/Chaiudbbhd/ci-cd_demo

📸 Project Architecture
🔜 Coming Soon

🔔 Future Improvements
GitHub Webhook Integration for automatic build triggers

Slack Notifications in Jenkinsfile

Kubernetes monitoring using Prometheus + Grafana

Auto-scaling and load balancing

🙌 Let's Connect
If you're interested in DevOps, Cloud, or Automation, feel free to connect with me on LinkedIn.
Happy to collaborate and share learning!

🔖 Hashtags
#DevOps #Java #SpringBoot #Docker #Kubernetes #Jenkins #CI_CD #CloudComputing #Automation
