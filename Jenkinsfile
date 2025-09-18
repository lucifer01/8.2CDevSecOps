pipeline {
    agent any
    triggers {
        pollSCM('H/2 * * * *') // checks on every 2 minute, and builds only if there are changes in git repo
    }
    stages {
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
                // Ensure coverage report exists
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true' // This will show known CVEs in the output
            }
        }
    }
}
