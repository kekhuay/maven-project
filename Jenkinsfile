pipeline {
    agent any

    tools {
        maven 'local-maven'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message: 'Approve Staging Deployment?'
                }
                build job: 'deploy-to-staging'
            }
            post {
                success {
                    echo 'Code deployed to staging.'
                }
                failure {
                    echo 'Deployment failed.'
                }
            }
        }
    }
}
