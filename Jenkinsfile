
pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials-id' // ID de las credenciales Docker en Jenkins
        DOCKER_IMAGE = 'jhonuel/myapp'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jhonuel/devops-automation.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    def dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Aquí puedes agregar tus comandos de prueba
                    sh '''
                        echo "Running tests..."
                        # Ejemplo de pruebas
                        docker run --rm ${DOCKER_IMAGE}:${env.BUILD_ID} sh -c "echo 'Tests passed!'"
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('', "${DOCKER_CREDENTIALS_ID}") {
                        def dockerImage = docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}")
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh '''
                        echo "Deploying container..."
                        docker run -d --name devops-automation -p 81:80 ${DOCKER_IMAGE}:${env.BUILD_ID}
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
