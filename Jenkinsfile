pipeline {
    agent any
    tools {
        nodejs 'NodeJS' 
    }
    
    stages {
        // Stage 1: Install dependencies
        stage('Install') {
            steps { 
                sh 'npm install' 
            }
        }
        
        // Stage 2: Run tests with email alerts
        stage('Test') {
            steps { 
                sh 'npm test' 
            }
            post {
                failure {
                    emailext (
                        subject: '🚨 Tests Failed in Build ${BUILD_NUMBER}',
                        body: """
                        <h2>Test Failure Alert</h2>
                        <p><b>Build URL:</b> <a href="${BUILD_URL}">${BUILD_URL}</a></p>
                        <p><b>Render Site:</b> https://gallery-ynej.onrender.com</p>
                        <pre>${currentBuild.rawBuild.getLog(100).join('\n')}</pre>
                        """,
                        to: 'kihiu253@gmail.com',
                        mimeType: 'text/html'
                    )
                }
            }
        }
        
        // Stage 3: Deploy (Render handles this automatically)
        stage('Deploy') {
            steps { 
                sh 'node server.js' 
                echo 'Render will auto-deploy to https://gallery-ynej.onrender.com'
            }
        }
        
        // Stage 4: Slack notification (Milestone 4)
        stage('Notify Slack') {
            steps {
                slackSend(
                    channel: '#KELVIN_IP1',
                    message: """
                    ✅ *Deployment Successful* 
                    *Build #*: ${BUILD_NUMBER}
                    *Live Site*: https://gallery-ynej.onrender.com
                    *Details*: ${BUILD_URL}console
                    """,
                    color: 'good',
                    tokenCredentialId: 'slack-webhook-token' // Store in Jenkins credentials
                )
            }
        }
    }
    
    triggers {
        pollSCM('* * * * *') // Check for changes every minute
    }
}