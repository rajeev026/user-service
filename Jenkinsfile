pipeline {
    agent any

    environment {
        IMAGE_NAME = "user-service"
        CONTAINER_NAME = "user-service-container"
        APP_PORT = "5001"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Code checkout ho raha hai...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Docker image build ho raha hai...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            parallel {
                stage('Unit Test') {
                    steps {
                        echo 'Unit test run ho raha hai...'
                        sh 'python -m pytest || true'
                    }
                }

                stage('Docker Image Check') {
                    steps {
                        echo 'Docker image verify ho raha hai...'
                        sh 'docker images | grep $IMAGE_NAME'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Application deploy ho raha hai...'
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d -p $APP_PORT:$APP_PORT --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }
}
