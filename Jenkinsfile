pipeline {
    agent { label 'jenkins-agent' }
    tools {
        jdk 'java17'
        maven 'maven3'
    }

    environment {
        SONARQUBE_URL = 'http://54.157.166.207:9000'
        SONARQUBE_PROJECT_KEY = 'regester'
        SONARQUBE_LOGIN_TOKEN = credentials('sonar-token')
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonar-token') {
                        sh '''
                            mvn sonar:sonar \
                                -Dsonar.projectKey=${SONARQUBE_PROJECT_KEY} \
                                -Dsonar.host.url=${SONARQUBE_URL} \
                                -Dsonar.login=${SONARQUBE_LOGIN_TOKEN} \
                                -Dsonar.java.binaries=.
                        '''
                    }
                }
            }
        }
    }
}
