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
                // Ensure coverage report exists
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true' // This will show known CVEs in the output
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
            curl -L -o sonar-scanner-cli-7.2.0.5079-macosx-aarch64.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.2.0.5079-macosx-aarch64.zip
            unzip -o sonar-scanner-cli-7.2.0.5079-macosx-aarch64.zip
            ./sonar-scanner-7.2.0.5079-macosx-aarch64/bin/sonar-scanner
            '''
                }
            }
        }
    }
}
