pipeline {
  agent any
  tools{
        jdk 'jdk11'
        maven 'maven'
    }

  stages {
      stage('Build Artifact') {
            steps {
              //sh "echo passed" 
               sh "mvn clean package -DskipTests=true"
               archiveArtifacts artifacts: 'target/*.jar' //so that they can be downloaded later
            }
        }  

        stage('Unit test - Junit and Jacoco') {
            steps {
              sh "mvn test"

            }
           post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco (execPattern : 'target/jacoco.exec' )
              }
            } 
         }
        }   
    }
}

