pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'printenv'
                sh 'docker build -t jhonuel/myapp:"$BUILD_ID" .'
           //     sh 'docker build . jhonuel/myapp:"$BUILD_ID" .'
            }
        }

        stage('Publish') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-credentials-id', url: '']) {
                    sh 'docker push jhonuel/myapp:"$BUILD_ID"'
                }
            }
        }
    }
}
