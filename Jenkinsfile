pipeline {
    agent any
    tools{
     maven 'LocalMaven'
    }
    parameters {
        string(name: 'tomcat_dev', defaultValue: '34.238.124.177', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '54.173.18.67', description: 'Production Server')
    }
    triggers {
         pollSCM('* * * * * ')
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
            stage ('Deploy to Staging') {
                steps {
                    sh "scp -i /Users/simpowkin/Documents/JenkinsDemo/tomcat-demo.pem.txt **/target/*.war ec2-user@${parameters.tomcat_dev}:/var/lib/tomcat7/webapps"

                }
            }
            stage ("Deploy to Production") {
                steps {
                     sh "scp -i /Users/simpowkin/Documents/JenkinsDemo/tomcat-demo.pem.txt **/target/*.war ec2-user@${parameters.tomcat_prod}:/var/lib/tomcat7/webapps"
                 }
              }
          }
      }
   }
}
