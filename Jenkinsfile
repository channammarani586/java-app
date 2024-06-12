pipeline {
    agent { label 'jenkins-agent' }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    environment {
        APP_NAME = "register-app"
        RELEASE = "1.0.0"
        DOCKER_USER = "gkamalakar06"
        DOCKER_PASS = "docker-hub"
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages {
        stage('Clean workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'gitaccess', url: 'https://github.com/kamalakar22/register-app.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test Application') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_USER, DOCKER_PASS) {
                        docker_image = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                        docker_image.push()
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}
