pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('Dockerid')
        DOCKERHUB_REPO = 'jhonuel/myapp'
        SSH_CREDENTIALS_ID = 'ssh-credentials-id'
        IMAGE_TAG = "latest"
        SERVER_USER = 'dockeradmin'
        SERVER_HOST = '192.168.50.118'
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
                    sh '''
                    ssh ${SERVER_USER}@${SERVER_HOST} << EOF
                    docker pull ${DOCKERHUB_REPO}:${IMAGE_TAG}
                    docker stop your-container || true
                    docker rm your-container || true
                    docker run -d --name your-container -p 81:80 ${DOCKERHUB_REPO}:${IMAGE_TAG}
                    EOF
                    '''
                }
            }
        }
    }
}
