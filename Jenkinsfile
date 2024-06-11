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
               sh "mvn clean package"
            }
        }

	stage('Test Application') {
            steps {
               sh "mvn test"
            }
        }
    }
}
