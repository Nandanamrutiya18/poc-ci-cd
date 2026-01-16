pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Unit Tests (CI)') {
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t poc-python-app .'
            }
        }

        stage('Deploy on Server (CD)') {
            steps {
                sh '''
                docker rm -f poc-python || true
                docker run -d --name poc-python poc-python-app
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline Completed Successfully'
        }
    }
}
