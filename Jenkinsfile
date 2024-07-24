pipeline {
    agent any

    stages {
        stage('Verify Docker') {
            steps {
                sh 'docker --version || { echo "Docker ready"; exit 1; }'
           }
        }
        stage('Build') {
            steps {
                sh 'printenv'
                sh 'docker build -t jhonuel/myapp:"$BUILD_ID" .'
            }
        }

        stage('Publish') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub', url: '']) {
                    sh 'docker push jhonuel/myapp:"$BUILD_ID"'
                }
            }
        }
    }
}
