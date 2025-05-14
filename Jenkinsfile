pipeline {
    agent any  // Runs on any available Jenkins agent

    stages {
        // Stage 1: Install dependencies
        stage('Install') {
            steps {
                sh 'npm install'  // Installs Node.js dependencies
            }
        }

        // Stage 2: Deploy to Render
        stage('Deploy') {
            steps {
                sh 'node server.js'  // Starts the server (Render will handle deployment)
            }
        }
    }

    // Trigger pipeline on Git push (via webhook or polling)
    triggers {
        pollSCM('* * * * *')  // Checks for changes every minute (temporary)
    }
}