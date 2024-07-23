pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        registry = "etiennedem12345/darinpope-dockerhub"
        registryCredential = "DockerID"
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean'
                sh 'mvn install'
                sh 'mvn package'
            }
        }
        stage('Test') {
            steps {
                echo 'test step'
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    docker.build registry +
