pipeline {
    environment {
        REPO_URL = 'https://github.com/jhonuel/devops-automation.git'
        BRANCH = 'dev04'
        IMAGE_NAME = 'jhonuel/myapp'
        DOCKERHUB_CREDENTIALS = 'Dockerid' // Jenkins credentials ID for DockerHub
        SERVER_SSH_CREDENTIALS = 'P@ssw0rd' // Jenkins credentials ID for server SSH
        SERVER_USER = 'dockeradmin'
        SERVER_IP = '192.168.50.118'
    }
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image ${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    newApp = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', "${DOCKERHUB_CREDENTIALS}") {
                        newApp.push("${env.BUILD_NUMBER}")
                        newApp.push('latest')
                    }
                }
            }
        }
        stage('Deploy to Server') {
            steps {
                script {
                    sshagent(credentials: ["${SERVER_SSH_CREDENTIALS}"]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ${SERVER_USER}@${SERVER_IP} << EOF
                        docker pull ${IMAGE_NAME}:${env.BUILD_NUMBER}
                        docker stop myapp || true
                        docker rm myapp || true
                        docker run -d --name myapp -p 80:80 ${IMAGE_NAME}:${env.BUILD_NUMBER}
                        EOF
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs() // Clean workspace after build
        }
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}
