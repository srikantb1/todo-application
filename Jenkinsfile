pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/srikantb1/todo-application.git',
                    branch: 'app-issue'
                )
            }
        }
        stage('Build with Maven') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    bat 'docker build -t todo-application-image:latest .'
                    echo "dckr_pat_FIsZePNH1DzR2lvWQCJmmWzEB3I" | bat 'docker login --username srikantb1 --password-stdin'
                    bat 'docker tag todo-application-image:latest srikantb1/todo-application-image:latest'
                    bat 'docker push srikantb1/todo-application-image:latest'
                    
                    }
                }
            }
        stage('Deploy with Docker Compose') {
            steps {
                bat 'docker compose up -d'
            }
        }
        stage('Verify Services') {
            steps {
                bat 'docker ps'
                bat 'curl http://localhost:8082' // Verify the application is running
            }
        }
        stage('Clean Workspace') {
            steps {
                bat 'rm -rf *' // Clean the workspace
            }
        }
    }
}