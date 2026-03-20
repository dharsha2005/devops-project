pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'dharshan080805/devops-app'
        DOCKER_TAG = 'latest'
    }
    
    stages {

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% ."
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat "echo %PASS% | docker login -u %USER% --password-stdin"
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                bat "docker push %DOCKER_IMAGE%:%DOCKER_TAG%"
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
            cleanWs()
        }
        failure {
            echo 'Pipeline failed!'
            cleanWs()
        }
    }
}
