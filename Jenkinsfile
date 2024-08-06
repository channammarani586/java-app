pipeline {
    agent { label 'Jenkins-Agent' }
    
    tools {
        jdk 'jdk'
        maven 'maven3'
    }
    
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', credentialsId: 'GITHUB', url: 'https://github.com/kamalakar22/register-app.git'
            }
        }
        
        // Add other stages as needed
    }
}

