# BookHub

# 📚 BookHub - DevOps CI/CD Project

## Overview

BookHub is a Spring Boot application created to demonstrate a complete DevOps CI/CD pipeline. The project automates code integration, quality analysis, containerization, and deployment using industry-standard DevOps tools.

This project was built as part of my DevOps learning journey to gain hands-on experience with Continuous Integration (CI) and Continuous Deployment (CD) practices.

---

## Tech Stack

### Backend

* Java 21
* Spring Boot 3
* Maven

### Version Control

* Git
* GitHub

### CI/CD

* Jenkins Pipeline
* GitHub Webhooks

### Code Quality

* SonarQube

### Containerization

* Docker

### Operating System

* Windows 11

---

## CI/CD Workflow

```text
Developer
    │
git push
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Webhook
    │
    ▼
Jenkins Pipeline
    │
    ▼
Checkout Source Code
    │
    ▼
Maven Build
    │
    ▼
JUnit Tests
    │
    ▼
SonarQube Analysis
    │
    ▼
Docker Image Build
    │
    ▼
Docker Container Deployment
```

---

## Features

* Automated build using Jenkins
* Automatic pipeline trigger through GitHub Webhooks
* Maven project build
* JUnit test execution
* Static code analysis with SonarQube
* Docker image creation
* Docker container deployment
* End-to-end CI pipeline

---

## Project Structure

```
BookHub/
│
├── src/
├── target/
├── Dockerfile
├── Jenkinsfile
├── pom.xml
├── mvnw
├── mvnw.cmd
└── README.md
```

---

## Tools Used

* Java 21
* Spring Boot
* Maven
* Git
* GitHub
* Jenkins
* SonarQube
* Docker

---

## Future Improvements

* Kubernetes Deployment
* Prometheus Monitoring
* Grafana Dashboard
* AWS Deployment
* Production-ready CI/CD Pipeline

---

## Author

**Suraj Deshmukh**

Aspiring DevOps Engineer passionate about Cloud Computing, Automation, CI/CD, and Containerizatio