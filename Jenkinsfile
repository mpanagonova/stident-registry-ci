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

                nohup npm start > app.log 2>&1 &
                echo $! > app.pid
                '''
            }
        }

        stage('Wait for app') {
            steps {
                sh '''
                export NVM_DIR="$HOME/.nvm"
                source $NVM_DIR/nvm.sh
                nvm use 24

                npx --yes wait-on http://localhost:8080 --timeout 30000
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
            if [ -f app.pid ]; then
              kill $(cat app.pid) || true
            fi
            '''
        }
    }
}