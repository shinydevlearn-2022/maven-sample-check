pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  environment {
        TOMCAT_HOME = '/home/shiny/apache-tomcat-9.0.97'
  }
  stages {
    stage('Initialize') {
      steps {
        sh '''
         echo "PATH = ${PATH}"
         echo "M2_HOME = ${M2_HOME}"
         echo "TOMCAT_HOME = ${TOMCAT_HOME}"
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
           cp target/*.war ${TOMCAT_HOME}/webapps/webapp.war
        '''
      }
    }
  }
}

