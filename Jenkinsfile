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
        sh 'docker run --rm ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -t https://www.google.com/
    }
}
