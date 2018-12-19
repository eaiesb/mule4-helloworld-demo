#!/usr/bin/groovy
pipeline {
    environment {
        JAVA_HOME = tool('java')
    }
agent any
options {
disableConcurrentBuilds()
}
stages {
  
stage("buildsrc") {
steps { buildsrc() 
      slackSend (message: 'mule4 deployed sucessfully')
      }
}
stage('upload to atifactory') {
    steps {
               def server = Artifactory.server 'artifactory'
               server.upload(uploadSpec)
    }
}  	
}
   post {
      failure {
            emailext attachLog: true, body: 'Deployment has failed', subject: 'Mule 4 Cloud Deployment Status', to: 'devops@eaiesb.com'
        }
      success {
              emailext attachLog: true, body: 'Deployment is completed', subject: 'Mule 4 Cloud Deployment Status', to: 'devops@eaiesb.com'
        }
  }
}
// steps
def buildsrc() {
dir ('.' ) {
    sh '/usr/maven/apache-maven-3.3.9/bin/mvn clean package deploy -DmuleDeploy'
}
}
def uploadSpec = """{
  "files": [
    {
      "pattern": "**/target/*.zip",
      "target": "generic-local"
    }
 ]
}"""

