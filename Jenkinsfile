pipeline {
    agent any

    tools {
        jdk 'jdk'
        maven 'maven3'
    }

    environment {
        // Define Docker Hub credentials and Kubernetes settings
        DOCKER_HUB_CREDENTIALS = 'docker-cred' // Jenkins credentials ID for Docker Hub
        DOCKER_IMAGE = 'channamma/happyapp:25' // Docker Hub username and repository
        KUBE_CONFIG = '/home/maste/.kube/config' // Path to your kubeconfig for Kubernetes access
    }

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', credentialsId: 'GITHUB', url: 'https://github.com/channammarani586/java-app.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t ${DOCKER_IMAGE}:latest .'
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    // Login to Docker Hub using credentials
                    withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the image to Docker Hub
                    sh 'docker push ${DOCKER_IMAGE}:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Set up Kubernetes access and deploy to "test" namespace
                    sh 'kubectl --kubeconfig=${KUBE_CONFIG} apply -f /home/maste/Desktop/deployment.yaml -n test'
                }
            }
        }
    }
}
