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
             script{
     def server = Artifactory.server 'artifactory'
     def uploadSpec = """{
  "files": [
    {
      "pattern": "**/target/*.zip",
      "target": "generic-local/mulehelloworld"
    }
 ]
}"""                 
              def buildInfo1 = server.upload spec: uploadSpec
              server.publishBuildInfo buildInfo1
             }
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


