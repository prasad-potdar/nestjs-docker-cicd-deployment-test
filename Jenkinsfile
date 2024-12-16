pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nest-cicd-deployment-test-app'
        // Correctly append /usr/local/bin to the PATH
        PATH = "/usr/local/bin:$PATH"
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
                    sh "docker build -t ${IMAGE_NAME}:${env.BUILD_NUMBER} ."
                    sh "docker tag ${IMAGE_NAME}:${env.BUILD_NUMBER} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Run Container Locally') {
            steps {
                script {
                    sh """
                        docker stop ${IMAGE_NAME} || true
                        docker rm ${IMAGE_NAME} || true
                    """
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
