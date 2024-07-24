pipeline {
    agent any

    stages {
        stage('Install Docker') {
            steps {
                sh '''
                curl -fsSL https://get.docker.com -o get-docker.sh
                sh get-docker.sh
                '''
            }
        }
        stage('Test Docker') {
            steps {
                sh 'docker --version'
            }
        }
        stage('Checkout SCM') {
            steps {
                checkout scmGit(branches: [[name: '*/dev04']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jhonuel/devops-automation.git']])
            }
        }
    }
}
