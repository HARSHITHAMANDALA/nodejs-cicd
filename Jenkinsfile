pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t nodejs-status-api:latest .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                bat 'docker stop nodejs-status-container || exit 0'
                bat 'docker rm nodejs-status-container || exit 0'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker run -d --name nodejs-status-container -p 3000:3000 nodejs-status-api:latest'
            }
        }

        stage('Test API') {
            steps {
                bat 'curl http://localhost:3000/status'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed!'
        }
    }
}