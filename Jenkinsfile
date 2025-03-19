pipeline {
    agent any

    environment {
        IMAGE_NAME = "divyadharshini11"
        REGISTRY = "docker.io"
        DOCKER_CREDENTIALS_ID = "divyadharshini11"
        GITHUB_CREDENTIALS_ID = "DivyadharshiniA"
        APP_DIR = "/home/vboxuser/jenkins"

    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: GITHUB_CREDENTIALS_ID, url: https://github.com/DivyadharshiniA/jenkins.git, branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "cd $APP_DIR && docker build -t $IMAGE_NAME:latest ."
                }
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }

        stage('Push Image to Docker Registry') {
            steps {
                script {
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }

        stage('Deploy using Docker Compose') {
            steps {
                script {
                    sh "cd $APP_DIR && docker-compose down && docker-compose up -d"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully! '
        }
        failure {
            echo 'Pipeline failed! Check the logs for errors.'
        }
    }
}
