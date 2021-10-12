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
    
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/L4or1/TestWebapp.git > trufflehog'
        sh 'cat trufflehog'
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
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.123.55.56:/opt/tomcat/webapps/WebApp.war'
              }      
           }       
  }
 }
}
