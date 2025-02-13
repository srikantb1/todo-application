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
                    // Hardcoded credentials (NOT RECOMMENDED)
                    def dockerRegistry = "https://index.docker.io/v1/"
                    def dockerUsername = "srikantb1"
                    def dockerPassword = "dckr_pat_FIsZePNH1DzR2lvWQCJmmWzEB3I"
                    def dockerImageName = "todo-application-image"
                    def dockerImageTag = "latest"

                    // Log in to Docker registry
                    sh "echo ${dockerPassword} | docker login -u ${dockerUsername} --password-stdin ${dockerRegistry}"

                    // Build Docker image
                    sh "docker build -t ${dockerUsername}/${dockerImageName}:${dockerImageTag} ."

                    // Push Docker image to the registry
                    sh "docker push ${dockerUsername}/${dockerImageName}:${dockerImageTag}"
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose down'
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
