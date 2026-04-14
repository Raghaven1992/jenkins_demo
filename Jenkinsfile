pipeline {
    agent any

    environment {
        IMAGE_NAME     = "welcome-demo"
        CONTAINER_NAME = "welcome-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Generate Welcome Page') {
            steps {
                echo "Generating HTML for user: ${env.USER_NAME}"

                bat '''
                powershell -Command ^
                "(Get-Content index.html) -replace '__NAME__', '%USER_NAME%' | Set-Content index.html"
                '''

                bat 'type index.html'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."

                bat '''
                docker build -t %IMAGE_NAME%:latest .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Deploying container..."

                bat '''
                docker rm -f %CONTAINER_NAME% 2>nul
                docker run -d --name %CONTAINER_NAME% -p 8080:80 %IMAGE_NAME%:latest
                '''
            }
        }

        stage('Success') {
            steps {
                echo "✅ Deployment completed successfully!"
                echo "🌐 Open browser: http://localhost:8080"
            }
        }
    }

    post {
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
``
