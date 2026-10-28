pipeline {
    agent any

    environment {
        IMAGE_NAME = "anshulverma45/my-web-project"
    }

    stages {
        stage('Pull Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh '''
                        echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push "$IMAGE_NAME:latest"
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                    docker pull "$IMAGE_NAME:latest"
                    docker rm -f my-web-container || true
                    docker run -d --name my-web-container --restart unless-stopped -p 8081:80 "$IMAGE_NAME:latest"
                    sleep 3
                    curl -f http://localhost:8081/
                '''
            }
        }
    }
}
