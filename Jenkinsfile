pipeline {
    agent any

    environment {
        // Jenkins credentials ID containing Docker Hub username + password
        DOCKERHUB = credentials('dockerhub-login')

        // Static username for image names
        DOCKERHUB_USERNAME = "chilukurir"

        BACKEND_IMAGE = "${DOCKERHUB_USERNAME}/mean-backend:latest"
        FRONTEND_IMAGE = "${DOCKERHUB_USERNAME}/mean-frontend:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Rajesh-210/crud-dd-task-mean-app.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh '''
                  cd backend
                  docker build -t ${BACKEND_IMAGE} .
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh '''
                  cd frontend
                  docker build -t ${FRONTEND_IMAGE} .
                '''
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                sh '''
                  echo "${DOCKERHUB_PSW}" | docker login -u "${DOCKERHUB_USR}" --password-stdin
                  docker push ${BACKEND_IMAGE}
                  docker push ${FRONTEND_IMAGE}
                '''
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh '''
                  # Use the docker-compose.yml in Jenkins workspace
                  docker compose pull
                  docker compose up -d
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
