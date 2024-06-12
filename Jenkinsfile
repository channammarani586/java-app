pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'java17'
        maven 'maven3'
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
        // stage('Static Code Analysis') {
        //     environment {
        //         SONAR_URL = "http://34.232.78.48:9000"
        //     }
        //     steps {
        //         withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
        //             sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
        //         }
        //     }
        // }
        stage('Build & Push Docker Image') {
            environment {
                APP_NAME = "register-app"
                RELEASE = "1.0.0"
                DOCKER_USER = "gkamalakar06"
                IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
                IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
                REGISTRY_CREDENTIALS = credentials('docker-hub')
            }
            steps {
                script {
                    def dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
