pipeline {
    agent any

    environment {
        IMAGE_NAME = "browserhub"
        CONTAINER_NAME = "browserhub"
    }

    stages {

        stage('Verify Workspace') {
            steps {
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                -p 8081:80 \
                $IMAGE_NAME
                '''
            }
        }

        stage('Check Docker') {
            steps {
                sh '''
                docker ps
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }

        stage('Verify Kubernetes') {
            steps {
                sh '''
                kubectl get pods
                kubectl get svc
                '''
            }
        }
    }

    post {

        success {
            echo '========================================='
            echo ' BrowserHub deployed successfully!'
            echo ' Docker : http://localhost:8081'
            echo '========================================='
        }

        failure {
            echo 'Pipeline Failed!'
        }

        always {
            sh 'docker ps || true'
            sh 'kubectl get pods || true'
        }
    }
}
