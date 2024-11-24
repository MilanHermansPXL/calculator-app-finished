pipeline {
    agent any
    tools {
        nodejs 'TINnode-devops'
    }
    triggers {
        cron('0 14 * * 5')
    }
    stages {
        stage('tmp') {
            steps {
                sh 'rm -rf *'
                sh 'rm -rf bundle bundle.zip junit.xml'
            }
        }
        stage('fetching source') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:MilanHermansPXL/calculator-app-finished.git'
            }
        }
        stage('install dependencies') {
            steps {
                // Verwijder node_modules en package-lock.json om alles schoon te starten
                sh 'rm -rf node_modules package-lock.json'

                // Installeer alleen express
                sh 'npm install express'
            }
        }

        stage('unit test') {
            steps {
                sh 'npm test --reporters=jest-junit'
                junit 'junit.xml'
            }
        }
        stage("create bundle") {
            steps {
                sh 'mkdir -p bundle' 
                sh '''
                rsync -av --exclude=".git" --exclude="bundle" --exclude=".gitignore" --exclude="README.md" --exclude="Jenkinsfile" --exclude="test/" ./ bundle/
                '''
                sh 'zip -r bundle.zip bundle' 
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'junit.xml', allowEmptyArchive: true 

            archiveArtifacts artifacts: 'bundle.zip', allowEmptyArchive: false
        }
        failure {
            
            sh 'echo "Pipeline poging faalt op $(date)" >> ~/jenkinserrorlog'
        }
    }
}