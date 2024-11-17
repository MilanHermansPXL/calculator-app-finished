pipeline {
    agent any
    tools {
        nodejs 'TINnode-devops' // Zorg dat de Node.js installatie correct geconfigureerd is in Jenkins
    }
    stages {
        stage('Install dependencies') {
            steps {
                sh 'npm install' // Installeer alle afhankelijkheden gedefinieerd in package.json
            }
        }
        stage('Clone main repository') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:PXL-2TIN-Devops-2425/ci-in-jenkins-team-zoutkorrel.git'
            }
        }
        stage('Fetch calculator source') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:MilanHermansPXL/calculator-app-finished.git'
            }
        }
        stage('Run unit tests') {
            steps {
                sh 'npm test tests/calculator.test.js' // Voer de unittests uit
            }
        }
        stage('Run JUnit tests') {
            steps {
                sh 'mvn test' // Voer Maven tests uit
                junit 'target/surefire-reports/*.xml' // Verwerk JUnit-rapporten
            }
        }
    }
}