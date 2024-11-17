pipeline {
    agent any
    tools {
        nodejs 'TINnode-devops' // Zorg ervoor dat Node.js correct is ingesteld
    }
    stages {
        stage('Install dependencies') {
            steps {
                sh 'npm install' // Installeer Node.js afhankelijkheden
            }
        }
        stage('Run unit tests') {
            steps {
                sh 'npm test tests/calculator.test.js' // Voer de unittests uit
            }
        }
        stage('Publish JUnit reports') {
            steps {
                // Als je een JUnit XML-rapport hebt, kun je dat hier publiceren, bijvoorbeeld:
                junit '**/test-results.xml'
            }
        }
    }
}
