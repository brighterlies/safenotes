pipeline {
    agent any
    tools {
        nodejs "NodeJS"
    }
    stages {
        stage('Instalar Dependencias') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Análisis de Dependencias (OWASP Dependency-Check)') {
            steps {
                sh './dependency-check/bin/dependency-check.sh --scan . --format HTML --out reports/'
            }
        }
        
        stage('Escaneo de Contenedores (Trivy)') {
            steps {
                sh 'trivy image safenotes-app'
            }
        }
        
        stage('Pruebas de Seguridad Web (OWASP ZAP)') {
            steps {
                sh 'zap-cli quick-scan --self-contained http://localhost:3000'
            }
        }

        stage('Publicar Reportes') {
            steps {
                archiveArtifacts artifacts: 'reports/*.html', fingerprint: true
            }
        }
    }
}
