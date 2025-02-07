pipeline{
    agent any
    environment{
        DOCKER_IMAGE="todo-application-image"
        DOCKER_TAG="latest"
        DOCKER_HUB_CREDENTIALS="docker-hub-credentials"
    }
    stages{
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
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                withCredentials([usernamePassword(credentialsId:DOCKER_HUB_CREDENTIALS, usernameVariable:'DOCKER_USERNAME', passwordVariable:'DOCKER_PASSWORD')]){
                    sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_USERNAME}/${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    sh "docker push ${DOCKER_USERNAME}/${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
            }
        }
    }
    stage('Deploy with Docker Compose') {
        steps {
            sh 'docker compose up -d'
        }
    }
    stage('Verify Services'){
        steps {
            sh 'docker ps'
            sh 'curl http://localhost:8082'
        }
    }
    stage('Clean Workspace'){
        steps {
            sh 'rm -rf*'
        }
    }
    }

}
