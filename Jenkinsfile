pipeline {
    agent any

    triggers {
        // SCM polling every 5 minutes
        pollSCM('H/5 * * * *')
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
                    // Windows uses 'bat' instead of 'sh'
                    def changes = bat(script: 'git diff HEAD^ HEAD --name-only', returnStdout: true).trim()
                    echo "Changed files: ${changes}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (fileExists('index.html')) {
                        echo "HTML file exists - validating..."
                        // Optional: Add validation steps here
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
                        echo "Content validation passed"
                    } else {
                        echo "No HTML file found, skipping test."
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying changes..."
                // Add your deployment steps here
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed! Check the logs for details."
        }
        always {
            cleanWs()
        }
    }
}
