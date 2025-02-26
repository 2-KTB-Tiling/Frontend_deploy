pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "luckyprice1103/tiling-frontend"
    }

    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'github_token', url: 'https://github.com/2-KTB-Tiling/Frontend_deploy.git'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', 
                    usernameVariable: 'DOCKER_HUB_USERNAME', 
                    passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    script {
                        sh 'echo "$DOCKER_HUB_PASSWORD" | docker login -u "$DOCKER_HUB_USERNAME" --password-stdin'
                    }
                }
            }
        }

        stage('Build & Push Frontend Image') {
            steps {
                script {
                    sh """
                    docker build -t ${DOCKER_HUB_REPO}:latest -f Dockerfile .
                    docker push ${DOCKER_HUB_REPO}:latest
                    """
                }
            }
        }
    }
}