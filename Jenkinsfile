pipeline {
    agent any

    tools {
        jdk 'JDK21'
    }

    environment {
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SurajDeshmukh69/BookHub.git'
            }
        }

        stage('Check Kubernetes') {
            steps {
                bat 'kubectl get nodes'
            }
        }

        stage('Build') {
            steps {
                bat 'mvnw.cmd clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat """
                    %SCANNER_HOME%\\bin\\sonar-scanner.bat ^
                    -Dsonar.projectKey=BookHub ^
                    -Dsonar.projectName=BookHub ^
                    -Dsonar.sources=src ^
                    -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t bookhub:v1 .'
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                docker stop bookhub-container || exit 0
                docker rm bookhub-container || exit 0
                docker run -d -p 9095:9095 --name bookhub-container bookhub:v1
                '''
            }
        }
    }
}