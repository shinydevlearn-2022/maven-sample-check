pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage('Initialize') {
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
    stage('Deploy-to-Tomcat') {
      steps {
        sh '''
           cp target/*.war /home/shiny/apache-tomcat-9.0.97/webapps/webapp.war
        '''
      }
    }
    stage('Restart-Tomcat') {
      steps {
        sh '''
           /home/shiny/apache-tomcat-9.0.97/bin/shutdown.sh || true
           /home/shiny/apache-tomcat-9.0.97/bin/startup.sh
        '''
      }
    }  
  }
}

