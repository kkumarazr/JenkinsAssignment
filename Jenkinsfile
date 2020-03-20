pipeline {
    agent any
    stages {
        stage("Checkout Source Code") {
            steps {            
              checkout scm
            }
        }
        stage("initial Setup"){
            steps{
                script{
            pom = readMavenPom file: 'pom.xml'
            PROJECT = pom.artifactId
            VERSION = pom.version
        }
        stage("Build ${PROJECT} and ${VERSION}") {
            steps{  
               sh "mvn clean install -DskipTests"
            }
        }
        stage("Awaiting for Approval deploying ${PROJECT} and ${VERSION}"){
           script{
             timeout(time: 5, unit: 'MINUTES') {
                 input( id: 'Deploy Id', message: 'Deploy environment?', ok: 'Deploy')
             }
           }
        }
        stage("Deploying ${PROJECT} and ${VERSION}") {
          steps{
            echo "Deploying the application"
          }
        }
        stage("Executing Tests") {
           steps{
             sh "curl http://localhost:<port#>/JenkinsAssignment/jenkinsapp >> jenkinsassignment.txt"
             sh "cat `$jenkinsassignment.txt`"
           }
        }
    }
}

