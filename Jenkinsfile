pipeline {
    agent any 
    tools {
        maven 'Maven'
        jdk 'Java 17'
    }
    environment {
        TOMCAT_HOME = '/home/shiny/apache-tomcat-9.0.97'
        REPO_URL = 'https://github.com/shinydevlearn-2022/maven-sample-check.git'
        BRANCH_NAME = 'master'
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout scm: [
                        $class: 'GitSCM',
                        branches: [[name: BRANCH_NAME]],
                        userRemoteConfigs: [[
                            url: REPO_URL,
                            credentialsId: '6121efbd-0dd6-43c2-90fb-f35fbc1a4f04'
                        ]]
                    ]
                }
            }
        }
        stage('Check-Git-Secrets') {
            steps {
                sh 'sudo docker run gesellix/trufflehog --json https://github.com/shinydevlearn-2022/maven-sample-check.git > trufflehog'
                sh 'cat trufflehog'
            }
        }
        stage('Source Compilation Analysis') {
            steps {
                sh 'wget "https://raw.githubusercontent.com/shinydevlearn-2022/maven-sample-check/master/owasp-dependency-check.sh" '
                sh 'chmod +x owasp-dependency-check.sh'
                sh 'sudo bash owasp-dependency-check.sh'
                sh 'cat odc-reports/dependency-check-report.xml'
            }
        }
        stage('SAST') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn sonar:sonar'
                    sh 'cat /home/shiny/simple-maven-project/target/sonar/report-task.txt'
                }
            }
        }    
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
