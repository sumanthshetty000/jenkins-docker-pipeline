# 🚀 CI/CD Pipeline with Jenkins, Docker & GitHub Webhooks on AWS EC2

## 📌 Project Overview

This project demonstrates a complete end-to-end CI/CD pipeline where a Dockerized web application is automatically built, pushed, and deployed on an AWS EC2 instance using Jenkins.

Whenever code is pushed to GitHub, a webhook triggers Jenkins, which builds a Docker image, pushes it to DockerHub, and deploys the updated container automatically.

---

## 🌐 Application Preview

The application is a simple web page styled using HTML and CSS, served via Nginx inside a Docker container.

### Features:

* Clean UI with header, content cards, and footer
* Displays project overview and technologies used
* Fully containerized and deployed via CI/CD

---

## 🧱 Tech Stack

* Jenkins (CI/CD Automation)
* Docker (Containerization)
* DockerHub (Image Registry)
* GitHub (Version Control + Webhooks)
* AWS EC2 (Deployment Server)
* Nginx (Web Server)
* Linux (Ubuntu)

---

## ⚙️ Architecture

```id="arch1"
Developer (git push)
        ↓
GitHub Repository
        ↓
Webhook Trigger
        ↓
Jenkins Pipeline
        ↓
Build Docker Image
        ↓
Push Image to DockerHub
        ↓
Deploy Container on EC2
        ↓
Application Live
```

---

## 🔄 CI/CD Workflow

1. Developer pushes code to GitHub
2. GitHub Webhook triggers Jenkins pipeline
3. Jenkins clones the repository
4. Docker image is built using Dockerfile
5. Image is pushed to DockerHub
6. Existing container is stopped and removed
7. New container is deployed automatically

---

## 🐳 Dockerfile

```dockerfile id="docker1"
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

---

## 📜 Jenkins Pipeline (Jenkinsfile)

```groovy id="jenkins1"
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sumanthsshetty/jenkins-cicd-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/sumanthshetty000/jenkins-docker-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop my-container || true
                docker rm my-container || true
                docker run -d -p 8086:80 --name my-container $DOCKER_IMAGE
                '''
            }
        }
    }
}
```

---

## 🌐 Application Access

```id="access1"
http://<EC2-PUBLIC-IP>:8086
```

---

## 🖥️ Application UI Details

The application displays:

* **Header** → Project title and deployment info
* **Project Overview Card** → Description of the application
* **Technologies Used Card** → Linux, Docker, AWS EC2, Nginx
* **Footer** → Author and year

---

## 🔐 Jenkins Setup Highlights

* Installed Jenkins on AWS EC2
* Configured Docker access for Jenkins user
* Added DockerHub credentials securely
* Configured GitHub Webhook for automation

---

## ⚠️ Challenges Faced & Solutions

### 1. Docker Permission Issue

**Error:** Permission denied while accessing Docker
**Fix:** Added Jenkins user to docker group

---

### 2. Port Conflict Issue

**Error:** Port already allocated
**Fix:** Stopped and removed existing containers before deployment

---

### 3. DockerHub Push Denied

**Error:** Access denied while pushing image
**Fix:** Corrected DockerHub username and used access token

---

### 4. Git Repository Issues

**Error:** Couldn't find revision
**Fix:** Verified branch name and repository URL

---

### 5. Webhook Trigger Issues

**Fix:** Verified webhook URL, Jenkins trigger settings, and network access

---

## 🎯 Key Learnings

* Built a complete CI/CD pipeline from scratch
* Understood Jenkins pipeline scripting
* Learned Docker image lifecycle (build → push → deploy)
* Implemented webhook-based automation
* Debugged real-world DevOps issues

---

## 📌 Future Improvements

* Add Docker Compose for multi-container setup
* Implement Nginx reverse proxy
* Deploy using Kubernetes
* Add monitoring tools (Prometheus & Grafana)

---

## 💼 Resume Description

Implemented an end-to-end CI/CD pipeline using Jenkins, Docker, and GitHub on AWS EC2. Automated Docker image build, push, and deployment using webhooks, enabling continuous delivery with minimal manual intervention.

---

## 👨‍💻 Author

**Sumanth S Shetty**
