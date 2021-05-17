pipeline {
  agent any
  tools { 
        maven 'localMaven' 
        jdk 'Local' 
    }
  triggers {
    pollSCM('*/5 * * * *')
  }
  stages{
    stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
       stage ('Build'){
        steps {
          sh 'mvn clean package'
        }
         post {
           success {
             echo 'Archiving...'
             archiveArtifacts artifacts:'**/target/*.war'
           }
         }
       }
       stage ('Deployments') {
         parallel{
           stage ('Deploy to Staging'){
             steps {
               sh "cp **/target/*.war /usr/local/Cellar/tomcat_stag/10.0.6/libexec/webapps"
             }
           }
           stage ('Deploy to prod') {
             steps {
               sh "cp **/target/*.war /usr/local/Cellar/tomcat_prod/10.0.6/libexec/webapps"
             }     
           }
         }
       }
    }
} 
  
