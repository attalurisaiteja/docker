pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "atalurisaiteja/devops"
        DOCKER_TAG   = "1.0.${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                        -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push('latest')
                }
            }
        }
    }

    post {
        success {
            echo "Docker image pushed successfully: ${DOCKER_IMAGE}:${DOCKER_TAG}"
        }
        cleanup {
            sh "docker image prune -f"
        }
    }
}
