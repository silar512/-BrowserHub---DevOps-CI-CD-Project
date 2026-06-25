pipeline {

    agent any

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {

        stage('Check Environment') {
            steps {
                sh 'echo $PATH'
                sh 'which docker'
                sh 'docker --version'
                sh 'which kubectl'
                sh 'kubectl version --client'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t browserhub .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f browserhub || true'
                sh 'docker run -d --name browserhub -p 8081:80 browserhub'
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
