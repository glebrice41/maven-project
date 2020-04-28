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
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
        steps {
            build job: 'deploy-to-staging'
            }
        }
    }
}
