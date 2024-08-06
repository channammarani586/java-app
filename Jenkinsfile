pipeline {
    agent {label'Jenkins-Agent'}
    tools{
        jdk 'jdk'
        maven3 'maven3'
    }
    stages{
        stage('SCM'){
            git branch: 'main', credentialsId: 'GITHUB', url: 'https://github.com/kamalakar22/register-app.git'
            cd register-app
        }
    }
}
