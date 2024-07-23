pipeline {
    agent any  // Usar cualquier agente disponible en Jenkins

    stages {
        stage('Test Docker') {
            steps {
                // Ejecutar el comando 'docker --version' para verificar que Docker está instalado y disponible
                sh 'docker --version'
            }
        }
    }
}
