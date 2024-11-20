pipeline {
    agent any
    tools {
        nodejs 'TINnode-devops'
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Checkout Source') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:MilanHermansPXL/calculator-app-finished.git'
            }
        }
        stage('Unit Test') {
            steps {
                sh 'npm test' // Voer de tests uit
                junit 'junit.xml' // Koppel het JUnit-rapport aan de build
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'junit.xml', allowEmptyArchive: true
        }
        success {
            echo 'Unit tests executed successfully!'
        }
        failure {
            echo 'Unit tests failed!'
        }
    }
}
