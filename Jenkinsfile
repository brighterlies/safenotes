pipeline {
    agent any
    tools {
        nodejs "NodeJS"
    }
    stages {
        stage('Instalar Dependencias') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Análisis de Dependencias (OWASP Dependency-Check)') {
            steps {
                bat 'C:\\Tools\\dependency-check\\bin\\dependency-check.bat --scan . --format HTML --out reports\\'
            }
        }
        
        stage('Escaneo de Contenedores (Trivy)') {
            steps {
                bat 'trivy image safenotes-app'
            }
        }
        
        stage('Pruebas de Seguridad Web (OWASP ZAP)') {
            steps {
                bat 'zap-cli quick-scan --self-contained http://localhost:3000'
            }
        }

        stage('Publicar Reportes') {
            steps {
                archiveArtifacts artifacts: 'reports\\*.html', fingerprint: true
            }
        }
    }
}
