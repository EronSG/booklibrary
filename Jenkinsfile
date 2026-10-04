pipeline {
    agent any

    environment {
        IMAGE_NAME = 'book-library-system'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'docker compose down --remove-orphans || true'
                sh 'docker compose up -d database'
                sh 'until [ "$(docker inspect -f "{{.State.Health.Status}}" $(docker compose ps -q database))" = "healthy" ]; do echo "Waiting for PostgreSQL..."; sleep 2; done'
                sh 'docker compose run --rm --user root web sh -c "npm install --include=dev && npm test"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
                sh 'docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose down --remove-orphans || true'
                sh 'docker compose up -d --build'
            }
        }

        stage('Smoke Test') {
            steps {
                sh 'docker compose ps'
                sh 'curl --fail http://host.docker.internal:8080/health'
            }
        }
    }

    post {
        always {
            sh 'docker compose ps || true'
        }
    }
}
