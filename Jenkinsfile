pipeline {
    agent {
        label 'jenkins_agent'
    }

    tools {
        nodejs 'NODEJS 7.8.0'
    }

    environment {
        IMAGE_NAME = 'nodemain'
        IMAGE_TAG = 'v1.0'
    } 

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Image') {
            steps {
               sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .' 
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d --expose 3000 -p 3000:3000 $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }

    post {
        failure {
            echo 'Main branch pipeline failed'
        }
        success {
            echo 'Main branch pipeline succeeded!'
        }
    }

}
