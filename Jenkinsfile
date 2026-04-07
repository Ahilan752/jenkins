pipeline {
    agent any

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
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t my-jenkins-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f my-jenkins-container || true
                docker run -d --name my-jenkins-container -p 3000:3000 my-jenkins-app
                '''
            }
        }
    }
}
