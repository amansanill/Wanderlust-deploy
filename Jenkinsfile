pipeline {
    agent any

    environment {
        BACKEND_IMAGE = "amansanill/masterslave-backend"
        FRONTEND_IMAGE = "amansanill/masterslave-frontend"
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

        stage("Scan Backend Image with Trivy") {
            steps {
                script {
                    sh """
                        docker image ls
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image masterslave-backend:latest
                    """
                }
            }
        }

        stage("Scan Frontend Image with Trivy") {
            steps {
                script {
                    sh """
                        docker image ls
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image masterslave-frontend:latest
                    """
                }
            }
        }

        stage("Tag & Push Backend and Frontend Images to Docker Hub") {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        sh """
                            # Tag and push backend
                            docker tag masterslave-backend:latest ${BACKEND_IMAGE}:${TAG}
                            docker push ${BACKEND_IMAGE}:${TAG}

                            # Tag and push frontend
                            docker tag masterslave-frontend:latest ${FRONTEND_IMAGE}:${TAG}
                            docker push ${FRONTEND_IMAGE}:${TAG}
                        """
                    }
                }
            }
        }
    }
}
