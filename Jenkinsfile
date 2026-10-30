
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code fetched from GitHub'
                sh 'ls -l'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t my-website:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Deploy Docker Container') {
            steps {
                sh '''
                    docker rm -f my-website-container || true

                    docker run -d \
                    --restart unless-stopped \
                    -p 8081:80 \
                    --name my-website-container \
                    my-website:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                sh '''
                    aws s3 cp index.html \
                    s3://simple-website-2026/index.html \
                    --content-type "text/html"
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    docker ps
                    curl -I http://localhost:8081
                    aws s3 ls s3://simple-website-2026/index.html
                '''
                echo 'Docker and S3 deployment completed!'
            }
        }
    }
}
