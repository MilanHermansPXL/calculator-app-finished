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
                sh 'npm test' 
                junit 'junit.xml' 
            }
        }
        stage("Create Bundle") {
            steps {
                sh 'mkdir -p bundle'
                sh 'cp -r package.json package-lock.json bundle/'
                sh 'zip -r bundle.zip bundle'
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
