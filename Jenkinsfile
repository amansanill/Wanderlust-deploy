pipeline {
    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/wanderlust"
        TAG = "v${BUILD_NUMBER}"
    }

    stages {
        stage("Clone Code") {
            steps {
                git url: "https://github.com/amansanill/Wanderlust-deploy.git", branch: "devops"
            }
        }

        stage("Deploy using Docker Compose") {
            steps {
                sh "docker-compose down || true"
                sh "docker-compose up -d --build"
            }
        }

        stage("Scan Docker Image with Trivy") {
            steps {
                script {
                    sh """
                        docker image ls
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ${IMAGE_NAME}:latest
                    """
                }
            }
        }

        stage("Tag & Push Docker Image to Docker Hub") {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        sh """
                            docker tag ${IMAGE_NAME}:latest ${IMAGE_NAME}:${TAG}
                            docker push ${IMAGE_NAME}:${TAG}
                        """
                    }
                }
            }
        }
    }
}
