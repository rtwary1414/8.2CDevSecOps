pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: ' https://github.com/rtwary1414/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
		export PATH=$PATH:/opt/homebrew/bin
		npm install'''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
		export PATH=$PATH:/opt/homebrew/bin
		npm test || true''' // Allows pipeline to continue despite test failures
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // Ensure coverage report exists
                sh '''
		export PATH=$PATH:/opt/homebrew/bin
		npm run coverage || true'''
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh '''
		export PATH=$PATH:/opt/homebrew/bin
		npm audit || true''' // This will show known CVEs in the output
            }
        }
    }
}
