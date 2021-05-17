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
       stage ('Deploy to Staging'){
         steps {
           build job:'deploy_to_stag'
         }
       }
       stage ('Deploy to prod') {
         steps {
           timeout(time:5, unit:"DAYS") {
            input message:'Approve Prod deployment?'
           }
           build job:'deploy_to_prod'
         }     
       } 
    }
} 
  
