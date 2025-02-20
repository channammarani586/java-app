pipeline {
    agent any  // This should be 'any' without the empty curly braces.

    tools {
        jdk 'jdk'
        maven 'maven3'
    }

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', credentialsId: 'GITHUB', url: 'https://github.com/PrajwalW07/register-app.git'
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
    }
}
