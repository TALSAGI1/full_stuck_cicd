pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'talsa'
        IMAGE_NAME = "flask-app"
        APP_VERSION = "latest"
    }

    stages {
        stage('Clone Code') {
            steps {
                echo "Cloning the repository..."
                git branch: 'main', url: 'https://github.com/TALSAGI1/full_stuck_cicd.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${APP_VERSION} ."
            }
        }
        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh "docker login -u ${DOCKERHUB_USER} -p ${DOCKERHUB_PASS}"
                    sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${APP_VERSION}"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes...'
                script {
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                }
            }
        }
    }
}
