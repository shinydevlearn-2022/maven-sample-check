pipeline {
    agent any 
    tools {
        maven 'Maven'
        jdk 'Java 17'
    }
    environment {
        TOMCAT_HOME = '/home/shiny/apache-tomcat-9.0.97'
    }
    stages {
        stage('Check Java') {
            steps {
                echo "Checking Java version..."
                sh 'java -version'
            }
        }    
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
                echo "Building the project using Maven"
                sh 'mvn clean package'
            }
        }
        stage('Deploy-to-Tomcat') {
            steps {
                echo "Deploying War file to Tomcat"
                sh '''
                sudo cp target/*.war ${TOMCAT_HOME}/webapps/simple-maven-project-1.0-SNAPSHOT.war
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
