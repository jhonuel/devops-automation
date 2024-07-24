pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('jenkins-pwd-01')
        DOCKERHUB_REPO = 'jhonuel/myapp'
        IMAGE_TAG = "latest"
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
                // Aquí puedes definir los pasos para desplegar la imagen en tu servidor
                // Esto podría variar dependiendo de cómo gestiones los despliegues (por ejemplo, docker-compose, kubectl, etc.)
                sh '''
                ssh user@your-server << EOF
                docker pull ${DOCKERHUB_REPO}:${IMAGE_TAG}
                docker stop myapp || true
                docker rm myapp || true
                docker run -d --name your-container ${DOCKERHUB_REPO}:${IMAGE_TAG}
                EOF
                '''
            }
        }
    }
}
