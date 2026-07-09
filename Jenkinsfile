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
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                        bat """
                        mvn clean verify sonar:sonar ^
                        -Dsonar.projectKey=BookHub ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.login=%SONAR_TOKEN%
                        """
                    }
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
                docker run -d --name bookhub-container -p 9095:9095 bookhub:v1
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '''
                kubectl rollout status deployment/bookhub-deployment
                kubectl get pods
                kubectl get services
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}