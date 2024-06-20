pipeline {
    agent any
    environment {
        leenewcredentials = credentials('leenewcredentials')
    }
    stages {
        stage('Deploy App') {
            steps {
                sh "kubectl apply -f flask-app.yaml"
                sh "sleep 50"
                sh "kubectl get svc flask-app"
            }
        }
        stage('Deploy NGINX') {
            steps {
                sh "kubectl apply -f nginx.yaml"
                sh "sleep 50"
                sh "kubectl get svc nginx"
            }
        }
    }
}