pipeline {
    agent any

    environment {
        // Use the credentials ID you created in Jenkins
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "kshitija1510/jenkins-pipeline-demo"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repo
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
                // Use DockerHub credentials from Jenkins
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Deploy (Optional)') {
            steps {
                echo "Deployment can be done here, e.g., docker run -d -p 5000:5000 $IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }

    post {
        always {
            echo "Pipeline finished"
        }
        success {
            echo "Docker image successfully pushed!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
