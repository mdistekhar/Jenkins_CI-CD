# Jenkins CI/CD Learning Project

## Overview

This project demonstrates how to build a **CI/CD pipeline using Jenkins**.
The pipeline automatically pulls code from a Git repository, builds the application, runs tests, and deploys it using Docker.

The purpose of this repository is to practice DevOps fundamentals including:

* Continuous Integration (CI)
* Continuous Delivery (CD)
* Jenkins Pipelines
* Docker containerization
* Automated builds and testing

---

# Tech Stack

| Tool    | Purpose                 |
| ------- | ----------------------- |
| Jenkins | CI/CD automation server |
| Git     | Version control         |
| Maven   | Build automation        |
| Docker  | Containerization        |
| Java    | Sample application      |

---

# Project Architecture

Developer → Git Repository → Jenkins Pipeline → Build → Test → Docker Image → Deployment

---

# Prerequisites

Before starting, install the following:

* Java JDK 11+
* Jenkins
* Git
* Docker
* Maven

Check installations:

```bash
java -version
git --version
docker --version
mvn -version
```

---

# Install Jenkins

## Ubuntu

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \
 /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
 https://pkg.jenkins.io/debian binary/ | sudo tee \
 /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```

Start Jenkins:

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Open in browser:

```
http://localhost:8080
```

Get admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# Jenkins Setup

1. Unlock Jenkins
2. Install **Suggested Plugins**
3. Create admin user
4. Open Jenkins Dashboard

---

# Create Jenkins Pipeline Job

1. Open Jenkins Dashboard
2. Click **New Item**
3. Enter project name
4. Select **Pipeline**
5. Save configuration

---

# Jenkinsfile

Create a file named `Jenkinsfile` in the repository.

```groovy
pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/yourusername/jenkins-cicd-project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-demo-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8081:8080 jenkins-demo-app'
            }
        }

    }
}
```

---

# Dockerfile

```dockerfile
FROM openjdk:11-jre-slim

WORKDIR /app

COPY target/*.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
```

---

# Running the Pipeline

1. Push code to GitHub
2. Jenkins detects changes
3. Pipeline starts automatically

Stages executed:

* Clone Repository
* Build Application
* Run Tests
* Build Docker Image
* Deploy Container

---

# Project Folder Structure

```
jenkins-cicd-project
│
├── Jenkinsfile
├── Dockerfile
├── pom.xml
├── README.md
│
└── src
    └── main
        └── java
```

---

# Useful Jenkins Plugins

Recommended plugins:

* Git Plugin
* Pipeline Plugin
* Docker Plugin
* Maven Integration Plugin
* Blue Ocean Plugin

---

# Example Pipeline Output

```
Stage: Clone Repository ✔
Stage: Build ✔
Stage: Test ✔
Stage: Build Docker Image ✔
Stage: Run Container ✔
```

Application runs at:

```
http://localhost:8081
```

---

# Learning Objectives

After completing this project you will understand:

* Jenkins installation and configuration
* Creating CI/CD pipelines
* Writing Jenkinsfile
* Integrating Git with Jenkins
* Building Docker images from pipelines
* Automating application deployment

---

# Future Improvements

Possible improvements for this project:

* Add Kubernetes deployment
* Implement automated security scanning
* Integrate Slack notifications
* Add automated rollback
* Use Jenkins shared libraries

---

# Author

DevOps Learning Project
Created for Jenkins CI/CD practice.
