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
          slackSend (color: "add8e6", message: 'Mule4-Helloworld Deployment Started')
          buildsrc() 
          slackSend (color: "0000ff", message: 'Mule4-Helloworld Build Sucessfully')
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
                      slackSend (color: "#FFA500",message: 'Mule4-Helloworld Artifacts Uploaded Sucessfully')
            }
    }
}  	
}
   post {
      failure {
            emailext attachLog: true, body: 'Deployment has failed', subject: 'Mule 4 Cloud Deployment Status', to: 'devops@eaiesb.com'
           slackSend (color: "#FF0000",message: 'Mule4-Helloworld Deployment Failed')
        }
      success {
              emailext attachLog: true, body: 'Deployment is completed', subject: 'Mule 4 Cloud Deployment Status', to: 'devops@eaiesb.com'
          slackSend (color: "#32CD32", message: 'Mule4-Helloworld Deployment is Sucessful')
        }
  }
}
// steps
def buildsrc() {
dir ('.' ) {
    sh '/usr/maven/apache-maven-3.3.9/bin/mvn clean package deploy -DmuleDeploy'
}
}


