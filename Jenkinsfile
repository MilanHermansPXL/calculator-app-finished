pipeline {
    agent any
    tools {
        nodejs 'TINnode-devops'
    }
    triggers {
        cron('0 14 * * 5') // Automatisch uitvoeren elke vrijdag om 14:00
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                sh 'rm -rf *'
                sh 'rm -rf bundle bundle.zip junit.xml'
            }
        }
        stage('Checkout Source') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:MilanHermansPXL/calculator-app-finished.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Unit Test') {
            steps {
                sh 'npm test --reporters=jest-junit'
                junit 'junit.xml'
            }
        }
        stage('create bundle'){
            steps{
                sh 'mkdir bundle'
                sh 'rsync -av --exclude="bundle" --exclude=".*" ./ bundle/'
                sh 'ls -l /bundle'
                sh 'zip bundle.zip bundle'
            }
        }

    }
    post {
        always {
            archiveArtifacts artifacts: 'junit.xml', allowEmptyArchive: true 
        }
        success {
            archiveArtifacts artifacts: 'bundle.zip', allowEmptyArchive: false
        }
        failure {
            sh 'echo "Pipeline poging faalt op $(date)" >> ~/jenkinserrorlog'
        }
    }
}
