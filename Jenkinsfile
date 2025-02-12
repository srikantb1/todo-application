pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/srikantb1/todo-application.git',
                    branch: 'master',
                )
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    sh 'docker build -t todo-application-image:latest .'
                    sh 'docker tag todo-application-image:latest <your-dockerhub-username>/todo-application:latest'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                        sh 'docker push <your-dockerhub-username>/todo-application:latest'
                    }
                }
            }
        }
    }
}