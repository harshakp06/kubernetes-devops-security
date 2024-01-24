pipeline {
  agent any
  tools{
        jdk 'jdk11'
        maven 'maven'
    }
  
  // below code removing old builds
 // options {
  // buildDiscarder logRotator(artifactDaysToKeepStr: '1', artifactNumToKeepStr: '1', daysToKeepStr: '1', numToKeepStr: '1')
// }


  stages {
      stage('Build Artifact') {
            steps {
            //  sh "echo passed" 
              sh "mvn clean package -DskipTests=true"
              archiveArtifacts artifacts: 'target/*.jar' //so that they can be downloaded later
              sh "echo $GIT_COMMIT --short HEAD"
            }
        }  

         stage('Unit test - Junit and Jacoco') {
            steps {
              sh "mvn test"
              
            }
          /* post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco (execPattern : 'target/jacoco.exec' )
              }
            } */ 
            
            } 

      /*   stage('SonarQube Analysis') {
               steps { 
               // def mvn = tool 'Default Maven';
              withSonarQubeEnv('sonarqube') {
                      sh "mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=numeric-application \
                            -Dsonar.projectName='numeric-application' \
                            -Dsonar.host.url=http://64.2129:9000 "
                                                    
              } 
              
            //  timeout(time: 2, unit: 'MINUTES') {
                script {

                  waitForQualityGate abortPipeline: true
                }
             //  } 

              }
             } */

           stage('Vulnerability Scan ') {
            steps {
                   sh "mvn dependency-check:check"
                }

           }
             
            /*  post {
              always {
                dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
              }
            }*/

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
        stage('GIT Push') {
            steps {
              script{
                  withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {

                    sh '''
                          
                          sed -i 's#replace#harshakp06/numeric-app:${GIT_COMMIT}#g' argocd/k8s_deployment_service.yaml
                          git add argocd/k8s_deployment_service.yaml
                          git commit -m "changes to image version"
                          git push -u origin main

                       '''
            }

          }
        }  
        }  



  }

        post {
            always {
              junit 'target/surefire-reports/*.xml'
              jacoco execPattern: 'target/jacoco.exec'
              // pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
              dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
            }

          }  
    
    
}

