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
                sh 'git clone -b main git@github.com:MilanHermansPXL/calculator-app-finished.git'
            }
        }
        stage('Unit Test') {
            steps {
                sh 'npm test' 
                sh 'junit junit.xml'
            }
        }
        stage("Create Bundle") {
            steps {
                // Maak de map "bundle"
                sh 'mkdir -p bundle'
                
                // Kopieer benodigde bestanden
                sh 'cp -r package.json bundle/'
                sh 'cp -r package-lock.json bundle/'

                // Maak een zip-bestand van de "bundle"-map
                sh 'zip -r bundle.zip bundle'

                // Controleer of de map correct gevuld is
                sh 'ls -a bundle'
            }
        }
    }
    post {
        always {
            // Archiveer de testresultaten
            archiveArtifacts artifacts: 'junit.xml', allowEmptyArchive: true
        }
        success {
            // Archiveer het zip-bestand als de pipeline slaagt
            archiveArtifacts artifacts: 'bundle.zip', allowEmptyArchive: false
            sh 'echo "Pipeline voltooid: bundle.zip is gearchiveerd."'
        }
        failure {
            // Schrijf foutmelding naar jenkinserrorlog
            sh 'echo "Pipeline poging faalt op $(date)" >> ~/jenkinserrorlog'
            sh 'echo "Pipeline is gefaald: foutmelding opgeslagen in jenkinserrorlog."'
        }
    }
}
