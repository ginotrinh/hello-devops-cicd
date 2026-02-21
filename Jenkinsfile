pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'your_dockerhub_username'
        IMAGE_NAME         = 'hello-devops'
    }

    stages {
        stage('Build Image') {
            steps {
                echo "🔨 Building Docker Image..."
                sh "docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest ."
                sh "docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${env.BUILD_ID} ."
            }
        }

        stage('Login') {
            steps {
                echo "🔑 Logging in to Docker Hub..."
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                }
            }
        }

        stage('Push Image') {
            steps {
                echo "🚀 Pushing Image to Docker Hub..."
                sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest"
                sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${env.BUILD_ID}"
            }
        }
    }

    post {
        always {
            echo "🧹 Cleaning up..."
            sh "docker logout"
        }
        success {
            echo "✅ Pipeline hoàn thành thành công! Image đã được đẩy lên Docker Hub."
        }
        failure {
            echo "❌ Pipeline thất bại! Vui lòng kiểm tra logs ở trên."
        }
    }
}
