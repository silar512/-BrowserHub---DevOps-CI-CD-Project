pipeline {

    agent any

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/silar512/-BrowserHub---DevOps-CI-CD-Project.git'
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
                sh 'docker run -d --name browserhub -p 8080:80 browserhub'
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
