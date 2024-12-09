pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
     stage ('Initialization') {
      steps {
        sh '''
               echo "PATH = ${PATH}"
               echo ""M2_HOME = ${M2-HOME}"
           '''
      }
     }     
     stage('Build') {
       sh 'mvn clean package'
     }
  }
}
