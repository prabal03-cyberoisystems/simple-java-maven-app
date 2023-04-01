pipeline {
    agent any
    tools {
        maven 'Maven' // specify the tool identifier
        snyk 'SNYK' //specify the tool identifier
    }
    environment {
    // Define the PATH environment variable to include the Snyk executable location
    PATH = "${tool 'SNYK'}/bin:${env.PATH}"
  }
    
    stages {
        stage('Build') {                           // Build stage 
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {                            // Test stage 
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Security Scan') {                    // snyk stage
            steps {
    withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_API_KEY')]) {
      sh '''
        export SNYK_TOKEN=$SNYK_API_KEY
        snyk test
        snyk monitor
      '''
         } 
      }
   }

        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
