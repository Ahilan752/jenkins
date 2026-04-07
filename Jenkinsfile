pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-jenkins-app"
        IMAGE_TAG  = "${BUILD_NUMBER}"
        DEPLOYMENT_NAME = "my-jenkins-app"
        CONTAINER_NAME  = "my-jenkins-app"
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
                sh 'minikube version'
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

        stage('Load Image to Minikube') {
            steps {
                sh '''
                minikube status
                minikube image load $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                kubectl set image deployment/$DEPLOYMENT_NAME $CONTAINER_NAME=$IMAGE_NAME:$IMAGE_TAG
                kubectl rollout status deployment/$DEPLOYMENT_NAME
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc'
            }
        }
    }
}
