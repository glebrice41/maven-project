pipeline {
    agent any

    tools {
        maven 'maven-3.6.3'
    }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
        steps {
            build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to Prod'){
            steps {
                 timeout(time:5, unit:'DAYS'){
                     input message: Approve PROD Deployment?

             build job: 'deploy-to-prod'
            }
            post{
                success{
                     echo 'Code deployed to Prod'
                }
                failure{
                     echo ' Deployment failed.'
                }
            }
        }
    }
}
