pipeline {
    agent any

    tools {
        jdk 'JDK21'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SurajDeshmukh69/BookHub.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvnw.cmd clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t bookhub:v1 .'
            }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker stop bookhub-container || exit /b 0'
            }
        }

        stage('Remove Old Container') {
            steps {
                bat 'docker rm bookhub-container || exit /b 0'
            }
        }
        
        stage('Run Container') {
            steps {
                bat 'docker run -d --restart unless-stopped -p 9095:9095 --name bookhub-container bookhub:v1'
            }
        }
    }
}