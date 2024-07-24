pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')  // ID de las credenciales de DockerHub
        DOCKERHUB_REPO = 'jhonuel/myapp'  // Reemplaza con tu repositorio en DockerHub
        IMAGE_TAG = "latest"
        SSH_CREDENTIALS_ID = 'ssh-credentials-id'  // ID de las credenciales SSH
        SERVER_USER = 'dockeradmin'  // Nombre de usuario del servidor remoto
        SERVER_HOST = '192.168.50.118'  // IP o nombre de dominio del servidor remoto
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scmGit(branches: [[name: '*/dev04']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jhonuel/devops-automation.git']])
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_REPO}:${IMAGE_TAG}")
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        docker.image("${DOCKERHUB_REPO}:${IMAGE_TAG}").push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                        sh '''
                        ssh -o StrictHostKeyChecking=no ${SERVER_USER}@${SERVER_HOST} << EOF
                        docker pull ${DOCKERHUB_REPO}:${IMAGE_TAG}
                        docker stop myapp-container || true
                        docker rm myapp-container || true
                        docker run -d --name myapp-container -p 81:80 ${DOCKERHUB_REPO}:${IMAGE_TAG}
                        EOF
                        '''
                    }
                }
            }
        }
    }
}
