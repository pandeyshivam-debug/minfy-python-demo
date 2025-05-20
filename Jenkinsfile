pipeline {
    agent any

    environment {
        IMAGE_NAME = "shivamdebug/minfy-python-demo"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Test') {
            steps {
                sh "docker run --rm ${IMAGE_NAME}:${IMAGE_TAG} python -m pytest tests/tests_main.py"
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Pull Image') {
            steps {
                sh "docker pull ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }

        stage('Run Container') {
            steps {
                sh "docker run --rm ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }
}
