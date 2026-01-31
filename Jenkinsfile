pipeline {
    agent any

    environment {
        IMAGE_NAME = "poc-python-app"
        CONTAINER_NAME = "poc-python-app"
        PORT = "5000"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

       stage('Verify Python & Install Dependencies (CI)') {
    steps {
        bat '''
        python --version
        python -m pip install --upgrade pip
        python -m pip install -r requirements.txt
        '''
        }
    }

        stage('Run Unit Tests (CI)') {
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image (CI)') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:latest .
                '''
            }
        }

        stage('Deploy on Server (CD)') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d -p $PORT:5000 --name $CONTAINER_NAME $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD Pipeline Completed Successfully'
        }
        failure {
            echo '❌ CI/CD Pipeline Failed'
        }
        always {
            echo '🔁 Pipeline Execution Finished'
        }
    }
}
