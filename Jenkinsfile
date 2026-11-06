pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                echo 'Pulling source code from GitHub...'
                checkout scm
            }
        }

        stage('Build Frontend & Backend') {
            steps {
                echo 'Building MERN components...'
                sh '''
                    echo "Checking client directory..."
                    cd client && npm install || true
                    cd ..
                    echo "Checking server directory..."
                    cd server && npm install || true
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker Images...'
                // You can add your docker build commands here once Dockerfiles are ready
            }
        }
    }
}
