pipeline {
    agent any
    tools {
        nodejs 'TINnode-devops'
    }
    stages {
        stage('install dependencies'){
            steps{
                sh 'npm install express'
            }
        }
        stage('Hello') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:PXL-2TIN-Devops-2425/ci-in-jenkins-team-zoutkorrel.git'
            }
        }
        stage('fetching source'){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:MilanHermansPXL/calculator-app-finished.git'
            }
        }
        stage("unittest"){
            steps{
                sh 'npm test tests/calculator.test.js'
            }
        }
    }
}