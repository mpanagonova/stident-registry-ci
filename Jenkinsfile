pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                bat 'npm ci'
            }
        }

        stage('Start app (background)') {
            steps {
                bat 'start /B cmd /c npm start'
            }
        }

        stage('Wait for app') {
            steps {
                bat 'npx --yes wait-on http://localhost:8080'
            }
        }

        stage('Run tests') {
            steps {
                bat 'npm test'
            }
        }
    }

    post {
        always {
            // Cleanup so the port isn't left occupied after the build
            bat 'npx --yes kill-port 8080'
        }
    }
}