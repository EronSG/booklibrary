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
                sh 'npm test'
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
                sh 'curl --fail http://localhost:8080/health'
            }
        }
    }

    post {
        always {
            sh 'docker compose ps || true'
        }
    }
}
