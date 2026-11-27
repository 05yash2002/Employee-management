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
                    set -e
                    echo "Installing & building client..."
                    (cd client && npm install && npm run build)
                    echo "Installing server dependencies..."
                    (cd server && npm install)
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker Images...'
                // Next step: add Dockerfiles & image build / push / Ansible deploy
            }
        }
    }
}
