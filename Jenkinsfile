
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Getting source code from GitHub'
                sh 'ls -l'
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
                    aws s3 ls \
                    s3://simple-website-2026/index.html
                '''
                echo 'Deployment completed successfully!'
            }
        }
    }
}
