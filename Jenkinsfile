pipeline {
    agent any

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
    }
}
