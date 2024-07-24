#!/bin/bash

# Variables
REPO_URL="https://github.com/jhonuel/devops-automation.git"
DOCKER_IMAGE="jhonuel/myapp"
CREDENTIALS_ID="docker-hub"

# Clonar el repositorio
git clone $REPO_URL repo
cd repo

# Verificar si Docker está instalado
if ! command -v docker &> /dev/null
then
    echo "Docker no está instalado. Instalando Docker..."
    sudo apt-get update
    sudo apt-get install -y docker.io
    sudo systemctl start docker
    sudo systemctl enable docker
    sudo usermod -aG docker $USER
    newgrp docker
fi

# Verificar la versión de Docker
docker --version

# Crear un archivo Jenkinsfile para el pipeline
cat <<EOL > Jenkinsfile
pipeline {
    agent any

    stages {
        stage('Verify Docker') {
            steps {
                sh 'docker --version || { echo "Docker is not installed"; ex
