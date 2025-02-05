pipeline {
    agent any

    environment {
        // Usa el mismo nombre que configuraste en Jenkins: "SonarQube"
        SONAR_SCANNER_HOME = tool 'SonarQube'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Nombre que configuraste en Jenkins
                    withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_AUTH_TOKEN')]) {
                    sh '''
    ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
    -Dsonar.projectKey=MyProject \
    -Dsonar.sources=. \
    -Dsonar.host.url=http://localhost:9000 \
    -Dsonar.login=$SONAR_AUTH_TOKEN
'''
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline failed due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                
                // Descargar la imagen de ZAP
                sh 'docker pull ghcr.io/zaproxy/zaproxy:stable'

                // Ejecutar el escaneo contra una URL específica
                script {
                    def scanStatus = sh(script: 'docker run --rm ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -t https://www.google.com', returnStatus: true)

                    if (scanStatus != 0) {
                        error "Security scan failed with status: ${scanStatus}"
                    }
                }
            }
        }
    }
}

