pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "kshitija1510/jenkins-pipeline-demo"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "jenkins-demo"
        APP_PORT = "5000"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Docker Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Deploy') {
            steps {
                // Stop existing container if running
                sh "docker stop $CONTAINER_NAME || true"
                sh "docker rm $CONTAINER_NAME || true"
                // Run new container
                sh "docker run -d -p $APP_PORT:$APP_PORT --name $CONTAINER_NAME $IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }

    post {
        always {
            echo "Pipeline finished"
        }
        success {
            echo "Docker image pushed and deployed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
