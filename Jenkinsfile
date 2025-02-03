pipeline {
    agent any
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
    }
}
stage('Security Scan') {
    steps {
        echo 'Running security scan...'
        sh 'docker run --rm owasp/zap2docker-stable zap-baseline.py -t http://localhost:8080
    }
}
