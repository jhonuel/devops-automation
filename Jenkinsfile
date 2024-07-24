pipeline {
    agent any

    stages {
        stage('Print Environment') {
            steps {
                sh 'env'
            }
        }
        stage('Test Docker') {
            steps {
                sh 'docker --version'
            }
        }
    }
}
