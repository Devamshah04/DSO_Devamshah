pipeline {
    agent any

    environment {
        registry = "devamshah04/myapp"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }

    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/Devamshah04/DSO_Devamshah.git'
            }
        }
        stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    // Example using Trivy for security scan
                    sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image ${registry}'
                }
            }
        }
        stage('Deploying Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Clean up') {
            steps {
                sh "docker rmi ${registry}"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
