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

        stage('Check Tools') {
            steps {
                sh 'docker --version'
                sh 'kubectl version --client'
                sh 'sudo -u ahilan minikube status'
            }
        }

        stage('Build Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest
                '''
            }
        }

        stage('Load Image into Minikube') {
            steps {
                sh '''
                sudo -u ahilan minikube image load $IMAGE_NAME:$IMAGE_TAG
                sudo -u ahilan minikube image load $IMAGE_NAME:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                sudo -u ahilan kubectl apply -f deployment.yaml
                sudo -u ahilan kubectl apply -f service.yaml
                '''
            }
        }
    }
}
