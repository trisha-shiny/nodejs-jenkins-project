pipeline {
    agent any

    environment {
        IMAGE_NAME = "nodejs-devops-app"
        DOCKER_HUB = "reshma0209"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag $IMAGE_NAME:$TAG $DOCKER_HUB/$IMAGE_NAME:$TAG'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_HUB/$IMAGE_NAME:$TAG'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker rmi $DOCKER_HUB/$IMAGE_NAME:$TAG || true'
                sh 'docker rmi $IMAGE_NAME:$TAG || true'
            }
        }
    }
}
