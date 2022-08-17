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
     
    stage ('Source Composition Analysis') {
      steps {
        
            dependencyCheck additionalArguments: 'scan="https://github.com/rutvikshah2412/CICD-Pipeline-WebApp" --format HTML', odcInstallation: 'OWASP-Dependency-Check'
        
//          sh 'rm owasp* || true'
//          sh 'wget "https://raw.githubusercontent.com/rutvikshah2412/CICD-Pipeline-WebApp/main/owasp-dependency-check.sh" '
//          sh 'chmod +x owasp-dependency-check.sh'
//          sh 'bash owasp-dependency-check.sh'
//          sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
      }
    }
    
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/rutvikshah2412/CICD-Pipeline-WebApp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    stage ('Build') {
      steps {
        sh 'mvn clean package'
        }
      }
    
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@15.207.223.101:/home/ubuntu/prod/apache-tomcat-9.0.65/webapps/webapp.war'
              }      
           }       
    }
    
  }
}
