pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "divakars2005/html-app:latest"
        DOCKER_HUB_CREDENTIALS = "docker-cred"
    }

    stages {
        // Stage 1: Pull latest code from GitHub
        stage('Checkout Code') {
            steps {
                git 'https://github.com/divakar-srinivasan/Deploy.git'  // ✅ Updated GitHub repo URL
            }
        }

        // Stage 2: Build Docker Image
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE, ".")  // ✅ Fix build context issue
                }
            }
        }

        // Stage 3: Push Docker Image to Docker Hub
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh """
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                            docker push $DOCKER_IMAGE
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()  // ✅ Clean workspace after execution
        }
    }
}
