pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'kpranay2202@gmail.com'
        EMAIL_SUBJECT = 'Jenkins Pipeline Approval Needed'
        EMAIL_BODY = "Please confirm to proceed with the Jenkins pipeline by clicking the link below:\n\n${env.BUILD_URL}input"
    }

    stages {
        stage('Approval') {
            steps {
                script {
                    try {
                        // Send an email for approval
                        emailext (
                            subject: "${env.EMAIL_SUBJECT}",
                            body: "${env.EMAIL_BODY}",
                            to: "${env.EMAIL_RECIPIENT}",
                            mimeType: 'text/plain',
                            replyTo: 'noreply@example.com'
                        )
                        echo "Email sent successfully to ${env.EMAIL_RECIPIENT}"
                    } catch (Exception e) {
                        echo "Failed to send email: ${e.getMessage()}"
                        error("Stopping the pipeline due to email sending failure.")
                    }
                    
                    // Wait for confirmation
                    input message: 'Do you approve this pipeline to proceed?', ok: 'Yes, proceed'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                // Add your build steps here
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                // Add your test steps here
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deploy steps here
            }
        }
    }
}
