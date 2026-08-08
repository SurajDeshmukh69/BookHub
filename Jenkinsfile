pipeline {
    agent any

    ```
    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                        url: 'https://github.com/SurajDeshmukh69/BookHub.git'
            }
        }

        stage('Check Kubernetes') {
            steps {
                sh 'kubectl get nodes'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Check Jenkins User') {
            steps {
                sh '''
                whoami
                echo
                echo "USERPROFILE=$HOME"
                docker context ls
            '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    withCredentials([string(
                            credentialsId: 'sonar-token',
                            variable: 'SONAR_TOKEN'
                    )]) {
                        sh '''
                        ./mvnw clean verify sonar:sonar \
                        -Dsonar.projectKey=BookHub \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login="$SONAR_TOKEN"
                    '''
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t bookhub:v1 .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                        credentialsId: 'DockerHub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login \
                    -u "$DOCKER_USER" \
                    --password-stdin
                '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker tag bookhub:v1 surajgdeshmukh/bookhub:v1'
                sh 'docker push surajgdeshmukh/bookhub:v1'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop bookhub-container || true
                docker rm bookhub-container || true
                docker run -d \
                    --name bookhub-container \
                    -p 9095:9095 \
                    bookhub:v1
            '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
            '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
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
    ```

}
