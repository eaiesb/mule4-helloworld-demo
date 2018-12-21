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
          emailext mimeType: 'text/html' body: '''The following build details are as follows:<br> <br>
<table border="1">
<tr><td style="background-color:#33339F;color:white">,<b>Job Name</b></td><td>$JOB_NAME</td></tr>
<tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
<tr><td style="background-color:#33339F;color:white"><b>git url</b></td><td>$GIT_URL</td></tr>
<tr><td style="background-color:#33339F;color:white"><b>git commiter name</b></td><td>$GIT_COMMITTER_NAME</td></tr>
</table>
''', subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME}', to: 'devops@eaiesb.com'    
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


