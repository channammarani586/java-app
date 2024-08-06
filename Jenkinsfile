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
         stage("Trivy"){
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }

       }
        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
    }
}

