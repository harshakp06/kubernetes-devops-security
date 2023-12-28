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
               sh "echo $GIT_COMMIT --short HEAD"
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

         /* stage('Mutation Tests - PIT') {
            steps {
              sh "mvn org.pitest:pitest-maven:mutationCoverage"
              
            }
           post {
              always { */
               // pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'

             // pitmutation killRatioMustImprove: false, minimumKillRatio: 50.0, mutationStatsFile: '**/target/pit-reports/**/mutations.xml
           /*   }
            } 
            
            } */

          


          stage('Docker build and push') {
            steps {
              script{

                  withDockerRegistry(credentialsId: 'dockerhub') {

                    sh "printenv"
                    sh 'docker build -t harshakp06/numeric-app:""$GIT_COMMIT"" .'
                    sh 'docker push harshakp06/numeric-app:""$GIT_COMMIT""'
            }
          }
        }
        
         
        }   
    }
}

