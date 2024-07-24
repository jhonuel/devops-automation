pipeline {
    agent {
        label 'docker-enabled'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scmGit(branches: [[name: '*/dev04']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jhonuel/devops-automation.git']])
            }
        }
        stage('Test Docker') {
            steps {
                sh 'docker --version'
            }
        }
    }
}
