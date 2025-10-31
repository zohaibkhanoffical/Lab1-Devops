pipeline {
    agent any

    triggers {
        // Poll SCM every 5 minutes
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
                    def changes = sh(script: 'git diff HEAD^ HEAD --name-only', returnStdout: true).trim()
                    echo "Changed files: ${changes}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (fileExists('index.html')) {
                        echo "HTML file exists - validating..."
                        // You can add HTML validation here
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Check if index.html exists and contains required elements
                    if (fileExists('index.html')) {
                        def htmlContent = readFile('index.html')
                        if (!htmlContent.contains('Zohaib Khan')) {
                            error 'Required content not found in index.html'
                        }
                        echo "Content validation passed"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying changes..."
                // Add deployment steps here
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed! Check the logs for details."
        }
        always {
            // Clean workspace after build
            cleanWs()
        }
    }
}