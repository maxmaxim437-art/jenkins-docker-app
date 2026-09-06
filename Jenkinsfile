pipeline {

    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-credentials-id' 
        IMAGE_NAME = 'maxmaxim437/jenkins-docker-app' 
    }

    stages {
        stage('1. Checkout from GitHub') {
            steps {
                checkout scm
            }
        }

        stage('2. Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    app = docker.build("${env.IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('3. Push to DockerHub') {
            steps {
                script {
                    echo 'Pushing to DockerHub...'
                    docker.withRegistry('https://index.docker.io/v1/', "${env.DOCKERHUB_CREDENTIALS}") {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }

        stage('4. Deploy Application') {
            steps {
                script {
                    echo 'Deploying application locally...'
                    sh """
                        docker stop my-app || true
                        docker rm my-app || true
                        docker run -d --name my-app -p 5000:5000 ${env.IMAGE_NAME}:${env.BUILD_NUMBER}
                    """
                }
            }
        }
    }
} 
