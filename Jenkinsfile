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

        stage('Start Docker Container'){
             steps {
                bat 'docker run -d --restart unless-stopped -p 9095:9095 --name bookhub-container bookhub:v1'
             }

        }
    }
}