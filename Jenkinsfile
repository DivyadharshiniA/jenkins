pipeline {
    agent any

    environment {
        REGISTRY = "your-dockerhub-username" // Use the same DockerHub username
        IMAGE_NAME = "webapp"
        TAG = "latest"
        WORKDIR = "/opt/docker-kec" // Change the folder path based on where your Dockerfile and docker-compose.yml reside
    }

    stages {

        stage('Build & Push Docker Image') {
            steps {
                dir("$WORKDIR") {  
                    sh 'docker build -t $REGISTRY/$IMAGE_NAME:$TAG .'
                    withDockerRegistry([credentialsId: 'docker-hub-creds', url: '']) {
                        sh 'docker push $REGISTRY/$IMAGE_NAME:$TAG'
                    }
                }
            }
        }

        stage('Deploy Application') {
            steps {
                dir("$WORKDIR") {
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        success {
            echo 'Build, Push & Deploy Successful!'
        }
        failure {
            echo 'Pipeline Failed. Check logs!'
        }
    }
}

