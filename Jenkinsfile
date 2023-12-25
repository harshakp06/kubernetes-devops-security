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
               sh "mvn clean install -DskipTests=true"
               archiveArtifacts artifacts: 'target/*.jar' //so that they can be downloaded later
               sh "echo $GIT_COMMIT --short HEAD"
            }
        }  

        stage('Unit test - Junit and Jacoco') {
            steps {
              // sh "mvn test"

            }
           post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco (execPattern : 'target/jacoco.exec' )
              }
            } 
          
          stage('Docker build and push') {
            steps {
              withDockerRegistry([credentialsId: "dockerhub", url: ""])
              
               sh "printenv"
               sh 'docker build -t harshakp06/numeric-app:""$GIT_COMMIT"" .'
               sh 'docker push harshakp06/numeric-app:""$GIT_COMMIT""'
            }
        }
         
        }   
    }
}

