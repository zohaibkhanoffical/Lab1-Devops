pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') // Poll every 5 minutes
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Changes') {
            steps {
                script {
                    // Check if system is Windows or Linux
                    if (isUnix()) {
                        def changes = sh(script: 'git diff HEAD^ HEAD --name-only', returnStdout: true).trim()
                        echo "Changed files (Linux): ${changes}"
                    } else {
                        def changes = bat(script: 'git diff HEAD^ HEAD --name-only', returnStdout: true).trim()
                        echo "Changed files (Windows): ${changes}"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (fileExists('index.html')) {
                        echo "HTML file exists - validating..."
                    } else {
                        echo "No HTML file found, skipping validation."
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (fileExists('index.html')) {
                        def htmlContent = readFile('index.html')
                        if (!htmlContent.contains('Zohaib Khan')) {
                            error 'Required content not found in index.html'
                        }
                        echo "Content validation passed ✅"
                    } else {
                        echo "No HTML file found, skipping test."
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying changes..."
                // Add deploy logic here
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed! Check logs for details."
        }
        always {
            cleanWs()
        }
    }
}
