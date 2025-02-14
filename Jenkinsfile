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
        // stage('Build with Maven') {
        //     steps {
        //         bat 'mvn clean package -DskipTests'
        //     }
        // }
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
                    bat "echo ${dockerPassword} | docker login -u ${dockerUsername} --password-stdin ${dockerRegistry}"

                    // Build Docker image
                    bat "docker build -t ${dockerUsername}/${dockerImageName}:${dockerImageTag} ."

                    // Push Docker image to the registry
                    bat "docker push ${dockerUsername}/${dockerImageName}:${dockerImageTag}"
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                bat 'docker compose down'
                bat 'docker rm -f mysql-db'
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
        // stage('Build with Maven') {
        //     steps {
        //         sh 'mvn clean package -DskipTests'
        //     }
        // }
        // stage('Build and Push Docker Image') {
        //     steps {
        //         script {
        //             sh 'docker build -t todo-application-image:latest .'
        //             // echo "dckr_pat_FIsZePNH1DzR2lvWQCJmmWzEB3I" | sh 'docker login --username srikantb1 --password-stdin'
        //             // sh 'docker tag todo-application-image:latest srikantb1/todo-application-image:latest'
        //             // sh 'docker push srikantb1/todo-application-image:latest'
                    
        //             }
        //         }
        //     }
        // stage('Deploy with Docker Compose') {
        //     steps {
        //         sh 'docker compose up -d'
        //     }
        // }
        // stage('Verify Services') {
        //     steps {
        //         sh 'docker ps'
        //         sh 'curl http://localhost:8082' // Verify the application is running
        //     }
        // }
        // stage('Clean Workspace') {
        //     steps {
        //         sh 'rm -rf *' // Clean the workspace
        //     }
        // }
    }
}
