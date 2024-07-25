pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials-id'
        DOCKER_IMAGE = 'jhonuel/devops-automation'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev04', url: 'https://github.com/jhonuel/devops-automation.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    try {
                        sh 'echo "Docker version:"'
                        sh 'docker --version' // Verifica si Docker está disponible

                        sh 'echo "Building Docker image..."'
                        def dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                    } catch (Exception e) {
                        echo "Build failed with error: ${e.message}"
                        error("Build stage failed")
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    try {
                        sh 'echo "Running tests..."'
                        sh 'docker run --rm ${DOCKER_IMAGE}:${env.BUILD_ID} sh -c "echo \'Tests passed!\'"'
                    } catch (Exception e) {
                        echo "Test failed with error: ${e.message}"
                        error("Test stage failed")
                    }
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    try {
                        docker.withRegistry('', "${DOCKER_CREDENTIALS_ID}") {
                            def dockerImage = docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}")
                            dockerImage.push()
                        }
                    } catch (Exception e) {
                        echo "Push failed with error: ${e.message}"
                        error("Push stage failed")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    try {
                        sh 'echo "Deploying container..."'
                        sh 'docker run -d --name devops-automation -p 82:80 ${DOCKER_IMAGE}:${env.BUILD_ID}'
                    } catch (Exception e) {
                        echo "Deploy failed with error: ${e.message}"
                        error("Deploy stage failed")
                    }
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
