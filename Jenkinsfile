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
    
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/L4or1/TestWebapp/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
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
