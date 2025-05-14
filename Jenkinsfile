pipeline {
    agent any
    tools {
        nodejs 'NodeJS' 
    }
    stages {
        stage('Install') {
            steps { sh 'npm install' }
        }
        
        stage('Test') {
            steps { sh 'npm test' }
            post {
                failure {
                    emailext (
                        subject: '🚨 Tests Failed in Build 3304',
                        body: 'Check ${BUILD_URL}console',
                        to: 'kihiu253@gmail.com'
                    )
                }
            }
        }
        
        stage('Deploy') {
            steps { 
                sh 'node server.js' 
                echo 'Render will handle deployment'
            }
        }
    }
    triggers {
        pollSCM('* * * * *')
    }
}