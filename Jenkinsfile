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
                echo "Initializing environment variables"
                sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
                echo "TOMCAT_HOME = ${TOMCAT_HOME}"
                '''
            }
        }   
        stage('Build') {
            steps {   
                echo "Building project with Maven"
                sh 'mvn clean package'
            }
        }
        stage('Deploy-to-Tomcat') {
            steps {
                echo "Deploying to Tomcat"
                sh '''
                sudo cp target/*.war ${TOMCAT_HOME}/webapps/webapp.war
                '''
            }
        }
    }
    post {
        success {
            echo "Build and deployment completed successfully!"
        }
        failure {
            echo "Build or deployment failed. Check logs for details."
        }
    }
}
