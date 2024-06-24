pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'kpranay2202@gmail.com'
        EMAIL_SUBJECT = 'Jenkins Pipeline Approval Needed'
        EMAIL_BODY = "Please confirm to proceed with the Jenkins pipeline by clicking the link below:\n\n${env.BUILD_URL}input"
        CSV_FILE = 'pipeline_result.csv'
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
                script {
                    currentBuild.description = 'Build stage completed successfully.'
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                // Add your test steps here
                script {
                    currentBuild.description = 'Test stage completed successfully.'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deploy steps here
                script {
                    currentBuild.description = 'Deploy stage completed successfully.'
                }
            }
        }
    }

    post {
        always {
            script {
                // Create CSV content
                def csvContent = "Stage,Status\n"
                csvContent += "Approval,${currentBuild.result ?: 'SUCCESS'}\n"
                csvContent += "Build,${currentBuild.result ?: 'SUCCESS'}\n"
                csvContent += "Test,${currentBuild.result ?: 'SUCCESS'}\n"
                csvContent += "Deploy,${currentBuild.result ?: 'SUCCESS'}\n"
                
                // Write CSV content to file
                writeFile file: "${env.CSV_FILE}", text: csvContent

                // Archive the CSV file as a build artifact
                archiveArtifacts artifacts: "${env.CSV_FILE}", allowEmptyArchive: true
            }
        }
    }
}
