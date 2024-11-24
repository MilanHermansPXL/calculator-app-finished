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
        stage("Create Bundle") {
            steps {
                sh 'mkdir -p bundle'
                sh '''
                rsync -av \
                    --exclude=".git" \  // Uitsluiten van de .git folder
                    --exclude=".gitignore" \  // Uitsluiten van .gitignore
                    --exclude="README.md" \  // Uitsluiten van README.md
                    --exclude="Jenkinsfile" \  // Uitsluiten van de Jenkinsfile
                    --exclude="test/" \  // Uitsluiten van de test folder
                    ./ bundle/  // Kopieer alles naar de bundelmap, behalve bovenstaande bestanden
                '''
                sh 'zip -r bundle.zip bundle'
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

