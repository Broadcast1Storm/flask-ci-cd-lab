pipeline {
    agent any
    environment {
        IMAGE_NAME = "flask-ci-cd"
        APP_NAME = "flask-app"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }
        stage('Run Tests') {
            steps {
                sh "docker run --rm ${IMAGE_NAME}:latest pytest"
            }
        }
        stage('Deploy') {
            steps {
                sh """
                if [ \$(docker ps -aq -f name=${APP_NAME}) ]; then
                    docker rm -f ${APP_NAME}
                fi
                docker run -d --name ${APP_NAME} -p 8081:5000 ${IMAGE_NAME}:latest
                """
            }
        }
    }
}
