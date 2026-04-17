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
                sh '''
                export NVM_DIR="$HOME/.nvm"
                source $NVM_DIR/nvm.sh
                nvm use 24

                node -v
                npm -v
                npm ci
                '''
            }
        }

        stage('Start app (background)') {
            steps {
                sh '''
                export NVM_DIR="$HOME/.nvm"
                source $NVM_DIR/nvm.sh
                nvm use 24

                npm start > app.log 2>&1 &
                '''
            }
        }

        stage('Wait for app') {
            steps {
                sh '''
                export NVM_DIR="$HOME/.nvm"
                source $NVM_DIR/nvm.sh
                nvm use 24

                npx --yes wait-on http://127.0.0.1:8080
                '''
            }
        }

        stage('Run tests') {
            steps {
                sh '''
                export NVM_DIR="$HOME/.nvm"
                source $NVM_DIR/nvm.sh
                nvm use 24

                npm test
                '''
            }
        }
    }

    post {
        always {
            sh '''
            export NVM_DIR="$HOME/.nvm"
            source $NVM_DIR/nvm.sh
            nvm use 24

            npx --yes kill-port 8080 || true
            '''
        }
    }
}