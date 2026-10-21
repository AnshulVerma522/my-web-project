pipeline {
    agent any

    stages {
        stage('Pull Code from GitHub') {
            steps {
                checkout scm
                echo 'Code pulled successfully!'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-web-project:latest .'
            }
        }
    }
}
