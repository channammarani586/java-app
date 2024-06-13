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
        stage('Build and Push Docker Image') {
            environment {
                APP_NAME = "register-app"
                RELEASE = "1.0.0"
                DOCKER_USER = "gkamalakar06"
                IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
                IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
            }
            steps {
                script {
                    def dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                    withCredentials([string(credentialsId: 'dockerhub-cred', variable: 'DOCKERHUB_CRED')]) {
                        sh 'echo $DOCKERHUB_CRED | docker login -u $DOCKER_USER --password-stdin'
                        dockerImage.push("${IMAGE_TAG}")
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Trivy Scan') {
            steps {
                script {
                    sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ashfaque9x/register-app-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
                }
            }
        }
    }
}
