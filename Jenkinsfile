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
                sh '''
                    set -e
                    docker build -t employee-client:${BUILD_NUMBER} -t employee-client:latest ./client
                    docker build -t employee-server:${BUILD_NUMBER} -t employee-server:latest ./server
                '''
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing Docker Images to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        set -e
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag employee-client:${BUILD_NUMBER} ${DOCKER_USER}/employee-client:${BUILD_NUMBER}
                        docker tag employee-client:${BUILD_NUMBER} ${DOCKER_USER}/employee-client:latest
                        docker tag employee-server:${BUILD_NUMBER} ${DOCKER_USER}/employee-server:${BUILD_NUMBER}
                        docker tag employee-server:${BUILD_NUMBER} ${DOCKER_USER}/employee-server:latest
                        docker push ${DOCKER_USER}/employee-client:${BUILD_NUMBER}
                        docker push ${DOCKER_USER}/employee-client:latest
                        docker push ${DOCKER_USER}/employee-server:${BUILD_NUMBER}
                        docker push ${DOCKER_USER}/employee-server:latest
                    '''
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
