pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/srikantb1/todo-application.git',
                    branch: 'master'
                )
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    sh 'docker build -t todo-application-image:latest .'
                    sh 'docker tag todo-application-image:latest srikantb1/todo-application-image:latest'
                    sh 'docker push srikantb1/todo-application-image:latest'
                    echo "dckr_pat_FIsZePNH1DzR2lvWQCJmmWzEB3I" | sh 'docker login -u srikantb1'
                    }
                }
            }
        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose up -d'
            }
        }
        stage('Verify Services') {
            steps {
                sh 'docker ps'
                sh 'curl http://localhost:8082' // Verify the application is running
            }
        }
        stage('Clean Workspace') {
            steps {
                sh 'rm -rf *' // Clean the workspace
            }
        }
    }
}