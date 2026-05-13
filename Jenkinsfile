pipeline {
agent any
tools {
    nodejs 'NodeJS'
    }

stages {
   
    stage('Checkout') {
        steps {
            git branch: 'main', url: 'https://github.com/Thao-Lo/8.2CDevSecOps.git'
            }
        }
    stage('Install Dependencies') {
        steps {
            sh 'npm install'
        }
    }
    stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(
                    credentialsId: 'SONAR_TOKEN',
                    variable: 'SONAR_TOKEN'
                )]) {
                    sh '''
                    curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.1.0.4889-macosx-aarch64.zip

                    unzip -o sonar-scanner.zip

                    export PATH=$PATH:$(pwd)/sonar-scanner-7.1.0.4889-macosx-aarch64/bin

                    sonar-scanner
                    '''
                }
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