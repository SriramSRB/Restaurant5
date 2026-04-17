pipeline {
    agent any

    stages {
        stage ('1. Checkout code') {
            steps {
                checkout scm
            }
        }
        stage ('2. Build Docker Image') {
            steps {
                sh 'docker build -t restaurant5-app:latest .'
            }
        }
        stage ('3. Push Docker Image') {
            steps {
                withCredentials([string(credentialId: 'dockerhup_user', variable: 'DOCKER_PWD')]) {
                    sh 'echo "$DOCKER_PWD" | docker login -u sriramsrb --password-stdin'
                    sh 'docker push sriramsrb/restaurant5-app:latest'
                }
            }
        }
        stage ('4. Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl rollout restart deployment restaurant5-app-deployment'
            }
        }
    }
}