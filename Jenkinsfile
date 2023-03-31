pipeline {
    agent any
    tools {
        maven 'Maven' // specify the tool identifier
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Security Scan') {
            tools {
              snyk 'SNYK'
             }
            steps {
    withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_API_KEY')]) {
      sh '''
        export SNYK_TOKEN=$SNYK_API_KEY
        snyk test
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
