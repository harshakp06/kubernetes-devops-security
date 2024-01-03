# kubernetes-devops-security

## Fork and Clone this Repo

## Clone to Desktop and VM

## NodeJS Microservice - Docker Image -

`docker run -p 8787:5000 siddharth67/node-service:v1`

`curl localhost:8787/plusone/99`

## NodeJS Microservice - Kubernetes Deployment -

`kubectl create deploy node-app --image siddharth67/node-service:v1`

`kubectl expose deploy node-app --name node-service --port 5000 --type ClusterIP`

`curl node-service-ip:5000/plusone/99`

## Plugins

Blueocean

Maven - Maven can be enabled from the tools

Jacoco - for unit testing

Docker pipeline and Docker - for Docker push and for using Docker image in the pipeline

dependency-check-jenkins-plugin -

Sonar - Sonarcloud and Sonarcloud ToDo for the sonar code analysis  - pull and run sonarcube docker image on 9000 port

Sonar docker run
`` docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest ``



Code for Pitmutation Tests

```Jenkinsfile
stage('Mutation Tests - PIT') {
            steps {
              sh "mvn org.pitest:pitest-maven:mutationCoverage"
              
            }
           post {
              always { 
               pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'

              pitmutation killRatioMustImprove: false, minimumKillRatio: 50.0, mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
              }
            } 
            
            } 

```
