pipeline {
    agent any
    environment {
        OLD_VERSION = "v1"
        VERSION = "v2"
        jenkinscredentials = credentials('jenkinscredentials')
    }
    stages {
        stage('Build Images') {
            steps {
                sh "docker build -t ${jenkinscredentials_USR}/task1-flask-app:${VERSION} Task1"
                sh "docker build -t ${jenkinscredentials_USR}/task1-nginx:${VERSION} -f Task1/Dockerfile.nginx Task1"
            }
        }
        stage('Deploy Containers') {
            steps {
                sh "docker network create task1-net || true"
                sh "docker rm -f \$(docker ps -aq) || true"
                sh "docker run -d --network task1-net --name flask-app -e YOUR_NAME=Jenkins ${jenkinscredentials_USR}/task1-flask-app:${VERSION}"
                sh "docker run -d -p 80:80 --name nginx --network task1-net ${jenkinscredentials_USR}/task1-nginx:${VERSION}"
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh "docker login -u ${jenkinscredentials_USR} -p ${jenkinscredentials_PSW}"
                sh "docker push ${jenkinscredentials_USR}/task1-flask-app:${VERSION}"
                sh "docker push ${jenkinscredentials_USR}/task1-nginx:${VERSION}"
                sh "docker logout"
            }
        }
    }
    post {
        always {
            cleanWs()
            sh "docker rmi ${jenkinscredentials_USR}/task1-flask-app:${OLD_VERSION} || true"
            sh "docker rmi ${jenkinscredentials_USR}/task1-nginx:${OLD_VERSION} || true"

        }
    }
}