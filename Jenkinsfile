pipeline {
    agent {
        label 'jenkins_agent'
    }

    environment {
        IMAGE_TAG = 'v1.0'
    } 

    stages {
        stage('Set Variables') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        env.IMAGE_NAME = 'nodedev'
                        env.HOST_PORT = '3001'
                    } else {
                        env.IMAGE_NAME = 'nodemain'
                        env.HOST_PORT = '3000'
                    }
                }
            }
        }
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

        stage('Stop and Clean Previous Containers') {
            steps {
                script {
                    echo "Stopping and removing any existing container for image: ${IMAGE_NAME}"
                    sh '''
                        set +e
                        container_id=$(docker ps -q --filter "ancestor=${IMAGE_NAME}:${IMAGE_TAG}")
                        if [ -n "$container_id" ]; then
                            docker stop $container_id
                            docker rm $container_id
                        fi
                        set -e
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
                sh 'docker run -d --expose $HOST_PORT -p $HOST_PORT:3000 $IMAGE_NAME:$IMAGE_TAG'
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
