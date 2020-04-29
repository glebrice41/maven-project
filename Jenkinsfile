pipeline {
    agent any
      parameters {
         string(name: 'tomcat_dev', defaultValue: '34.219.153.51', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.222.149.178', description: 'Production Server')
    }

      triggers{
           pollSCM('* * * * *')
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
                      stage ('Deploy to Staging'){
                          steps {
                              sh "scp -i /Users/GlebRice41/app/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                          }
                      }

                      stage ("Deploy to Production"){
                          steps {
                              sh "scp -i /Users/GlebRice41/app/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                          }
                      }
                  }
              }
          }
      }
