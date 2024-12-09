pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub') 
        IMAGE_NAME = "whoami0709/jenkins-demo"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Pull latest code from Github') {
            steps {
                echo 'Pulling latest code from Github...'
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/22120074/Test-web.git'
            }
        }
        stage('Build and push Docker image') {
            steps {
                echo 'Building and pushing Docker image...'
                withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
                bat'''
                    docker build -t %IMAGE_NAME%:%IMAGE_TAG% .
                    docker push %IMAGE_NAME%:%IMAGE_TAG%
                '''
                }
            }
        }
        stage('Deploy docker image to docker container') {
            steps {
                echo 'Removing and stopping existing container with same name...'
                bat 'docker rm -f jenkins-demo || true'
                echo 'Deploying docker image to docker container...'
                bat 'docker run -d -p 8000:8000 --name jenkins-demo %IMAGE_NAME%:%IMAGE_TAG%'
            }
        }
    }
}