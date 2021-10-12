pipeline {
  agent any
  tools {
   maven 'Maven' 
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                  echo "PATH = ${PATH}"
                  echo "M2_HOME = ${M2_HOME}"
           '''
      }
    }
    
    stage('Build') {
      steps {
      sh 'mvn clean package' 
      }
    }
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['3.123.55.56']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.123.55.56:/opt/tomcat/webapps'
              }      
           }       
  }
 }
}
