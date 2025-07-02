pipeline {
    agent {
        label 'jenkins_agent'
    }

    environment {
        IMAGE_NAME = 'nodedev'
        IMAGE_TAG = 'v1.0'
    } 

    stages {
        stage('Install Dependencies and Test') {
            steps {
                nodejs(nodeJSInstallationName: 'node') {
                    sh '''
                        npm install
                        npm test
                    '''
                }
            }
        }

        stage('Build Image') {
            steps {
               sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .' 
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d --expose 3001 -p 3001:3000 $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }

    post {
        failure {
            echo 'Dev branch pipeline failed'
        }
        success {
            echo 'Dev branch pipeline succeeded!'
        }
    }

}
