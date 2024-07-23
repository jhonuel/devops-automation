pipeline {
    environment {
        IMAGEN = "jhonuel/myapp"
        USUARIO = 'Dockerid' // Jenkins credentials ID for DockerHub
        SERVER_SSH_CREDENTIALS = 'server-ssh-credentials-id' // Jenkins credentials ID for server SSH
        SERVER_USER = 'dockeradmin'
        SERVER_IP = '192.168.50.118'
    }
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: "dev04", url: 'https://github.com/jhonuel/devops-automation.git'
            }
        }
        stage('Verify Docker') {
            steps {
                sh 'docker --version'
                sh 'docker info'
            }
        }
        stage('Build') {
            steps {
                script {
                    newApp = docker.build("${env.IMAGEN}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image("${env.IMAGEN}:${env.BUILD_NUMBER}").inside('-u root') {
                        sh 'apache2ctl -v'
                    }
                }
            }
        }
        stage('Deploy to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', env.USUARIO) {
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
                        docker pull ${IMAGEN}:${env.BUILD_NUMBER}
                        docker stop myapp || true
                        docker rm myapp || true
                        docker run -d --name myapp -p 80:80 ${IMAGEN}:${env.BUILD_NUMBER}
                        EOF
                        """
                    }
                }
            }
        }
        stage('Clean Up') {
            steps {
                sh "docker rmi ${IMAGEN}:${env.BUILD_NUMBER}"
            }
        }
    }
}
