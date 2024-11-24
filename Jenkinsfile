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
                script {
                    // Maak de 'bundle' map
                    sh 'mkdir -p bundle'
                    
                    // Gebruik rsync om de gewenste bestanden te kopiëren
                    sh '''
                        rsync -av --exclude="*" --include="src/" --include="package.json" --include="package-lock.json" --include="config/**" ./ bundle/
                    '''
                    
                    // Maak een zip van de map 'bundle'
                    sh 'zip -r bundle.zip bundle'
                }
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
