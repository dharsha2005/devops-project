pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'dharshan080805/devops-app'
        DOCKER_TAG = 'latest'
    }
    
    stages {

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").push("${DOCKER_TAG}")
                }
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
