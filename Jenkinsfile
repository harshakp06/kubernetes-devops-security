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
               archive 'target/*.jar' //so that they can be downloaded later
            }
        }   
    }
}
