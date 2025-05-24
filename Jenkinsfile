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
            post {
                success {
                    emailext(
                        subject: "Test Stage SUCCESS: ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "The test stage completed successfully.\n\nConsole log: ${BUILD_URL}consoleText",
                        to: 'rtwary141@gmail.com',
                        attachmentsPattern: '**/build.log',
			attachLog: true
                    )
                }
                failure {
                    emailext(
                        subject: "Test Stage FAILURE: ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "The test stage failed.\n\nConsole log: ${BUILD_URL}consoleText",
                        to: 'rtwary141@gmail.com',
                        attachmentsPattern: '**/build.log',
			attachLog: true
                    )
                }
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
            post {
                success {
                    emailext(
                        subject: "Security Scan SUCCESS: ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "The security scan completed successfully.\n\nConsole log: ${BUILD_URL}consoleText",
                        to: 'rtwary141@gmail.com',
                        attachmentsPattern: '**/build.log',
			attachLog: true
                    )
                }
                failure {
                    emailext(
                        subject: "Security Scan FAILURE: ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "The security  scan failed.\n\nConsole log: ${BUILD_URL}consoleText",
                        to: 'rtwary141@gmail.com',
                        attachmentsPattern: '**/build.log',
			attachLog: true
                    )
                }
            }
        }
    }
}
