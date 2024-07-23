pipeline {
    environment {
        IMAGEN = "jhonuel/myapp"
        USUARIO = 'Dockerid'
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
        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('', env.USUARIO) {
                        newApp.push()
                    }
                }
            }
        }
        stage('Clean Up') {
            steps {
                sh "docker rmi ${env.IMAGEN}:${env.BUILD_NUMBER}"
            }
        }
    }
}
