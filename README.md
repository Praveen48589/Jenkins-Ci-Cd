# 🚀 Complete Jenkins CI/CD Setup Guide

A professional end-to-end Jenkins CI/CD setup guide including:

* Jenkins Installation
* Docker Installation
* Jenkins Initial Configuration
* Plugin Installation
* SMTP Email Setup
* Jenkins Shared Libraries
* Jenkins Worker/Agent Setup
* Docker Integration
* GitHub Integration
* Pipeline Configuration
* Best Practices
* Troubleshooting

---

# 📌 Table of Contents

1. Introduction
2. Prerequisites
3. Jenkins Installation
4. Docker Installation
5. Jenkins User Permissions
6. Access Jenkins Dashboard
7. Install Recommended Plugins
8. GitHub Integration
9. Configure SMTP Email Notifications
10. Create Jenkins Shared Libraries
11. Configure Jenkins Shared Library
12. Setup Jenkins Worker Agent
13. Connect Worker Agent to Jenkins Master
14. Create Jenkins Pipeline Job
15. Sample Jenkinsfile
16. Docker Compose Deployment
17. Best Practices
18. Troubleshooting
19. Conclusion

---

# 1️⃣ Introduction

This project demonstrates a production-style Jenkins CI/CD setup using:

* Jenkins
* Docker
* GitHub
* Shared Libraries
* SMTP Notifications
* Jenkins Worker Nodes
* Docker Compose

The setup is suitable for:

* DevOps Engineers
* CI/CD Automation
* Production Deployments
* Learning Real-World DevOps

---

# 2️⃣ Prerequisites

Before starting, ensure you have:

| Requirement       | Version     |
| ----------------- | ----------- |
| Ubuntu Server     | 22.04       |
| Java              | 17          |
| Docker            | Latest      |
| Jenkins           | Latest LTS  |
| GitHub Account    | Required    |
| Gmail Account     | Required    |
| EC2 Instance / VM | Recommended |

---

# 3️⃣ Jenkins Installation

## Step 1: Update System

```bash
sudo apt update -y
sudo apt upgrade -y
```

---

## Step 2: Install Java

```bash
sudo apt install openjdk-17-jdk -y
```

Verify Java:

```bash
java -version
```

---

## Step 3: Install Jenkins

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
```

```bash
sudo apt update
sudo apt install jenkins -y
```

---

## Step 4: Start Jenkins

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Check status:

```bash
sudo systemctl status jenkins
```

---

# 4️⃣ Docker Installation

## Install Docker

```bash
sudo apt install docker.io -y
```

Enable Docker:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Verify:

```bash
docker --version
```

---

# 5️⃣ Jenkins User Permissions

Add Jenkins user to Docker group:

```bash
sudo usermod -aG docker jenkins
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

Verify:

```bash
groups jenkins
```

---

# 6️⃣ Access Jenkins Dashboard

Open browser:

```text
http://YOUR_SERVER_IP:8080
```

Get initial password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Paste password into Jenkins setup screen.

---

# 7️⃣ Install Recommended Plugins

Navigate:

```text
Manage Jenkins → Plugins
```

Install:

| Plugin              |
| ------------------- |
| Docker Pipeline     |
| Pipeline            |
| Git                 |
| GitHub              |
| Blue Ocean          |
| Email Extension     |
| Pipeline Stage View |
| SSH Agent           |
| Credentials         |
| SSH Build Agents    |
| Workspace Cleanup   |

Restart Jenkins after plugin installation.

---

# 8️⃣ GitHub Integration

## Generate GitHub Token

Go to:

```text
GitHub → Settings → Developer Settings → Personal Access Tokens
```

Generate token with:

* repo
* workflow
* admin:repo_hook

---

## Add Credentials in Jenkins

Navigate:

```text
Manage Jenkins → Credentials → Global
```

Add:

* Username
* GitHub Token

---

# 9️⃣ Configure SMTP Email Notifications

## Enable Gmail App Password

Go to:

```text
Google Account → Security → 2-Step Verification
```

Enable:

* 2-Step Verification
* Generate App Password

---

## Configure SMTP in Jenkins

Navigate:

```text
Manage Jenkins → System
```

Under:

```text
E-mail Notification
```

Add:

| Field       | Value                                               |
| ----------- | --------------------------------------------------- |
| SMTP Server | smtp.gmail.com                                      |
| SMTP Port   | 465                                                 |
| Use SSL     | Enabled                                             |
| Username    | [your_email@gmail.com](mailto:your_email@gmail.com) |
| Password    | Gmail App Password                                  |

---

## Test Email

Click:

```text
Test configuration by sending test e-mail
```

If successful:

```text
Email sent successfully
```

---

# 🔟 Create Jenkins Shared Libraries

Shared Libraries help reuse common pipeline code.

---

## Project Structure

```text
jenkins-shared-library/
│
├── vars/
│   ├── dockerBuild.groovy
│   ├── dockerDeploy.groovy
│   └── notify.groovy
│
└── README.md
```

---

## dockerBuild.groovy

```groovy
def call(String imageName) {
    sh "docker build -t ${imageName} ."
}
```

---

## dockerDeploy.groovy

```groovy
def call() {
    sh 'docker compose down'
    sh 'docker compose up -d --build'
}
```

---

## notify.groovy

```groovy
def call(String status) {
    mail to: 'your_email@gmail.com',
    subject: "Build ${status}",
    body: "Pipeline finished with status: ${status}"
}
```

---

## Push Shared Library to GitHub

```bash
git init
git add .
git commit -m "shared library"
git remote add origin YOUR_REPO_URL
git push -u origin main
```

---

# 1️⃣1️⃣ Configure Jenkins Shared Library

Navigate:

```text
Manage Jenkins → System → Global Trusted Pipeline Libraries
```

Add:

| Field              | Value           |
| ------------------ | --------------- |
| Name               | my-shared-lib   |
| Default Version    | main            |
| Retrieval Method   | Modern SCM      |
| Source Code        | Git             |
| Project Repository | GitHub Repo URL |

Save configuration.

---

# 1️⃣2️⃣ Setup Jenkins Worker Agent

A Jenkins Agent executes builds separately from the Jenkins controller.

---

## Step 1: Create New Linux Machine

You can use:

* EC2 Instance
* Virtual Machine
* Another Ubuntu Server

---

## Step 2: Install Java on Agent

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

Verify:

```bash
java -version
```

---

## Step 3: Install Docker on Agent

```bash
sudo apt install docker.io -y
```

Enable Docker:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

---

## Step 4: Create Jenkins Agent User

```bash
sudo adduser jenkins
```

Add to Docker group:

```bash
sudo usermod -aG docker jenkins
```

---

## Step 5: Configure SSH Access

On Jenkins controller:

Generate SSH key:

```bash
ssh-keygen
```

Copy key to agent:

```bash
ssh-copy-id jenkins@AGENT_IP
```

---

# 1️⃣3️⃣ Connect Worker Agent to Jenkins Master

Navigate:

```text
Manage Jenkins → Nodes → New Node
```

Add:

| Field                 | Value                 |
| --------------------- | --------------------- |
| Node Name             | worker-agent          |
| Type                  | Permanent Agent       |
| Remote Root Directory | /home/jenkins         |
| Labels                | docker                |
| Launch Method         | Launch agents via SSH |
| Host                  | AGENT_IP              |
| Credentials           | SSH Credentials       |

Save node.

---

## Verify Agent

If connected successfully:

```text
Agent is online
```

---

# 1️⃣4️⃣ Create Jenkins Pipeline Job

Navigate:

```text
Dashboard → New Item
```

Select:

```text
Pipeline
```

---

## Configure GitHub Repository

Under:

```text
Pipeline → Pipeline Script from SCM
```

Choose:

* Git
* Repository URL
* Branch: main

---

# 1️⃣5️⃣ Sample Jenkinsfile

```groovy
@Library('my-shared-lib') _

pipeline {
    agent { label 'docker' }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/your-repo/project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                dockerBuild('my-app')
            }
        }

        stage('Deploy Application') {
            steps {
                dockerDeploy()
            }
        }
    }

    post {
        success {
            notify('SUCCESS')
        }

        failure {
            notify('FAILURE')
        }
    }
}
```

---

# 1️⃣6️⃣ Docker Compose Deployment

## docker-compose.yml

```yaml
services:
  frontend:
    build:
      context: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
    ports:
      - "3000:3000"
    depends_on:
      - mongodb
    environment:
      MONGO_URI: mongodb://admin:pass123@mongodb:27017

  mongodb:
    image: mongo
    ports:
      - "27017:27017"
```

---

## Deploy Application

```bash
docker compose up -d --build
```

Stop containers:

```bash
docker compose down
```

---

# 1️⃣7️⃣ Best Practices

## Security

* Use Jenkins Credentials Store
* Never hardcode passwords
* Use SSH authentication
* Restrict Jenkins ports

---

## CI/CD Practices

* Use Shared Libraries
* Keep pipelines modular
* Use worker agents for scalability
* Use Docker for consistency

---

## Monitoring

Recommended tools:

* Prometheus
* Grafana
* ELK Stack

---

# 1️⃣8️⃣ Troubleshooting

## Jenkins Cannot Access Docker

Fix:

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

## Permission Denied Workspace

Fix:

```bash
sudo chown -R jenkins:jenkins /var/lib/jenkins
```

---

## SMTP Email Not Working

Checklist:

* Use Gmail App Password
* Enable SSL
* Port 465
* Use correct email

---

## Agent Offline

Verify:

```bash
ssh jenkins@AGENT_IP
```

Check Java installed on agent.

---

# 1️⃣9️⃣ Conclusion

This Jenkins CI/CD setup provides:

✅ Automated CI/CD Pipelines
✅ Docker Integration
✅ Shared Library Reusability
✅ Email Notifications
✅ Scalable Worker Agents
✅ Production-Ready DevOps Workflow

This project is ideal for:

* DevOps Resume Projects
* Real-World CI/CD Learning
* Production Automation
* Jenkins Mastery

---

# ⭐ Future Improvements

You can extend this setup with:

* Kubernetes Deployment
* SonarQube Integration
* Trivy Security Scans
* Nexus Artifact Repository
* Terraform Infrastructure Automation
* AWS EKS Deployment
* GitHub Webhooks

---

# 👨‍💻 Author

Created for professional DevOps CI/CD learning and production deployment practice.

---

# 📜 License

This project is open-source and available for learning purposes.
