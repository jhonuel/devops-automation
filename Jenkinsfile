pipeline {
    agent {
        label 'docker-enabled'
    }

    stages {
        stage('Test Docker') {
            steps {
                sh 'docker --version'
            }
        }
    }
}
