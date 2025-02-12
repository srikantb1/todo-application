pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/srikantb1/todo-application.git'
        CREDENTIALS_ID = 'github' // Replace with your Jenkins credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: env.REPO_URL,
                    credentialsId: env.CREDENTIALS_ID,
                    branch: 'main' // Replace with your branch name
                )
            }
        }
    }
}
