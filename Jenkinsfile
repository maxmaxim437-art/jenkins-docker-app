

pipeline {

    agent any

    environment {
        DOCKER_IMAGE = "maximgonik/jenkins-docker-app"
        DOCKER_TAG   = "latest"
    }

    stages {

        stage('1. Checkout from GitHub') {
            steps {
                checkout scm
            }
        }

        stage('2. Build Docker Image') {
            steps {
                sh """
                    docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                """
            }
        }

        stage('3. Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                            -u "$DOCKER_USERNAME" \
                            --password-stdin

                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}

                        docker logout
                    '''
                }
            }
        }

        stage('4. Deploy Locally') {
            steps {
                sh """
                    docker stop jenkins-docker-app || true
                    docker rm jenkins-docker-app || true

                    docker pull ${DOCKER_IMAGE}:${DOCKER_TAG}

                    docker run -d \
                        --name jenkins-docker-app \
                        -p 5000:5000 \
                        --restart always \
                        ${DOCKER_IMAGE}:${DOCKER_TAG}
                """
            }
        }
    }
}
```

