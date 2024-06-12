pipeline {
    agent any
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
        stage('Static Code Analysis') {
          environment {
            SONAR_URL = "http://18.209.223.39:9000"
           }
            steps {
              withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
              sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
              }
           }
       }

    }
}
