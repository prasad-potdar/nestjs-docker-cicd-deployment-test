pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nest-cicd-deployment-test-app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/prasad-potdar/nestjs-docker-cicd-deployment-test.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image locally
                    sh "docker build -t ${IMAGE_NAME} ."
                    sh "docker tag ${IMAGE_NAME} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Run Container Locally') {
            steps {
                script {
                    // Stop and remove the old container if it exists
                    sh """
                        docker stop ${IMAGE_NAME} || true
                        docker rm ${IMAGE_NAME} || true
                    """
                    // Run the new container
                    sh "docker run -d --name ${IMAGE_NAME} -p 3000:3000 ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
