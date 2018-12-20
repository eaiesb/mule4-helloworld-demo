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
steps {
          slackSend (message: 'mule4-helloworld deployment started')
          buildsrc() 
          slackSend (message: 'mule4-helloworld build sucessfully')
      }
}

stage('upload to atifactory') {
    steps {
        script{
     def server = Artifactory.server 'artifactory'
     def uploadSpec = """{
  "files": [
    {
      "pattern": "**/*.jar",
      "target": "generic-local/mule4-demo-jars/mule4-helloworld-demo.jar"
    }
 ]
}"""                 
              def buildInfo1 = server.upload spec: uploadSpec
                      slackSend (message: 'mule4-helloworld artifacts uploaded sucessfully')
            }
    }
}  	
}
   post {
      failure {
            emailext attachLog: true, body: 'Deployment has failed', subject: 'Mule 4 Cloud Deployment Status', to: 'devops@eaiesb.com'
           slackSend (message: 'mule4-helloworld deployment failed')
        }
      success {
              emailext attachLog: true, body: 'Deployment is completed', subject: 'Mule 4 Cloud Deployment Status', to: 'devops@eaiesb.com'
          slackSend (message: 'mule4-helloworld deployment is sucessful')
        }
  }
}
// steps
def buildsrc() {
dir ('.' ) {
    sh '/usr/maven/apache-maven-3.3.9/bin/mvn clean package deploy -DmuleDeploy'
}
}


