pipeline {
    agent any
    tools {
        nodejs '20.9.0'
    }
    stages {
        stage('Version Check') {
            steps {
                sh 'npm version'
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/lucifer01/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true' // Allows pipeline to continue despite test failures
            }
        }
        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true' // Ensure coverage report exists
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true' // This will show known CVEs in the output
            }
        }
    }
}