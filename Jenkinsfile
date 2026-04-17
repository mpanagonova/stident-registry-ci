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
                sh 'npm ci'
            }
        }

        stage('Start app (background)') {
            steps {
                sh 'npm start > app.log 2>&1 &'
            }
        }

        stage('Wait for app') {
            steps {
                sh 'npx --yes wait-on http://127.0.0.1:8080'
            }
        }

        stage('Run tests') {
            steps {
                sh 'npm test'
            }
        }
    }

    post {
        always {
            sh 'npx --yes kill-port 8080 || true'
        }
    }
}