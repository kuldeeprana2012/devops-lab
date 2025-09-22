pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yourname/devops-lab.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-lab:latest .'
            }
        }

        stage('Test Build') {
            steps {
                sh 'docker run --rm devops-lab:latest node -v'
            }
        }

        stage('Deploy on VM1') {
            steps {
                sh '''
                docker stop devops-lab || true
                docker rm devops-lab || true
                docker run -d -p 3000:3000 --name devops-lab devops-lab:latest
                '''
            }
        }

        stage('Deploy on VM2') {
            agent { label 'vm2' }
            steps {
                sh '''
                docker stop devops-lab || true
                docker rm devops-lab || true
                docker run -d -p 3000:3000 --name devops-lab devops-lab:latest
                '''
            }
        }
    }
}
