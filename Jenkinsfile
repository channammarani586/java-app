pipeline {
    agent {label 'jenkins-agent'}
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
                sh "mvn clean package -DskipTests= true -e -X"
                sh '''mvn sonar:sonar -Dsonar.url=http://54.157.166.207:9000/ -Dsonar.login=sqa_eead51552f33dc25cd06166b550a07fe537c1114\
                      -Dsonar.java.binaries=. \
		              -Dsonar.projectKey=regester'''
            }
        }
    }
}
