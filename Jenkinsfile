pipeline {
    agent any
    tools {
        nodejs 'TINnode-devops'
    }
    triggers {
        cron('0 14 * * 5')
    }
    stages {
        stage('fetching source') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:MilanHermansPXL/calculator-app-finished.git'
            }
        }
        stage('install dependencies') {
            steps {
                sh 'npm install express'
            }
        }
        stage('unittest') {
            steps {
                sh 'npm test --reporters=jest-junit'
                junit 'junit.xml'
            }
        }
        stage("create bundle") {
            steps {
                
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'junit.xml', allowEmptyArchive: true 

            archiveArtifacts artifacts: 'bundle.zip', allowEmptyArchive: false

            sh 'rm -rf *'
        }
        failure {  
            sh 'echo "Pipeline poging faalt op $(date)" >> ~/jenkinserrorlog'
        }
    }
}