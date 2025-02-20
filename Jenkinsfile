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
                sh 'mvn clean install'
            }
        }

        stage('Docker build') {
            steps {
                sh 'docker build -t register-app .'
            }
        }
    }
}
