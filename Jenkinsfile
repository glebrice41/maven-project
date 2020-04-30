pipeline {
    agent any
      parameters {
         string(name: 'tomcat_dev', defaultValue: '54.149.229.173', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.190.1.125', description: 'Production Server')
     }

      triggers{
           pollSCM('* * * * *')
      }

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

              stage ('Deployments'){
                  parallel{
                      stage ('deploy-to-staging'){
                          steps {
                              sh "scp -i /Users/GlebRice41/app/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                          }
                      }

                      stage ('deploy-to-prod'){
                          steps {
                              sh "scp -i /Users/GlebRice41/app/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                          }
                      }
                  }
              }
          }
      }
