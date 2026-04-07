pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-jenkins-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Clone Check') {
            steps {
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Check Docker') {
            steps {
                sh 'docker --version'
                sh 'kubectl version --client'
            }
        }

        stage('Build Image') {
            steps {
                sh '''
                eval $(minikube -p minikube docker-env)
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
