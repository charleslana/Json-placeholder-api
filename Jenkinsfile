pipeline {
    agent any
    triggers {
        cron('H/2 * * * *') // Executa a cada 2 minutos
    }
    tools {
        nodejs 'Node 22.15.0'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/charleslana/Json-placeholder-api.git'
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Newman tests') {
            steps {
                sh 'npm run test-pipeline'
            }
        }
        stage('Publish Newman Report') {
            steps {
                archiveArtifacts artifacts: 'newman/**', allowEmptyArchive: true
            }
        }
        stage('Publish HTML Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'newman',
                    reportFiles: 'html-report.html',
                    reportName: 'Newman Test Report'
                ])
                junit 'newman/junit-report.xml'
            }
        }
    }
}