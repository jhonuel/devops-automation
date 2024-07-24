pipeline {
    agent {
        docker {
            image '24.0.7' // Usa una imagen Docker que tenga Docker instalado
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Monta el socket Docker del host
        }
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
