pipeline {
    agent any
 
    parameters {
        string(
            name: 'USER_NAME',
            defaultValue: 'Guest',
            description: 'Enter your name'
        )
    }
 
    environment {
        IMAGE_NAME = "welcome-demo"
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
                echo "Generating HTML for user: ${params.USER_NAME}"
 
                sh """
                sed -i 's/__NAME__/${params.USER_NAME}/g' index.html
                """
            }
        }
 
        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:latest .
                """
            }
        }
 
        stage('Run Docker Container') {
            steps {
                sh """
                docker rm -f ${CONTAINER_NAME} || true
                docker run -d \
                  --name ${CONTAINER_NAME} \
                  -p 8080:80 \
                  ${IMAGE_NAME}:latest
                """
            }
        }
 
        stage('Success') {
            steps {
                echo "✅ Application deployed successfully"
                echo "🌐 Open browser: http://localhost:8080"
            }
        }
    }
}
