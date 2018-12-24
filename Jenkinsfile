#!/usr/bin/groovy
pipeline {
    environment {
        JAVA_HOME = tool('java')
        GU = "${env.GIT_URL}"
    }
agent any
options {
disableConcurrentBuilds()
}
stages {
  
stage("buildsrc") {
steps {
          slackSend (color: "#f1502f", message: "Git URL is : ${env.GIT_URL}")
          slackSend (color: "#3e2c00", message: "GIT_COMMITTER_NAME is : ${env.GIT_COMMITTER_NAME}")
          slackSend (color: "#3e2c00", message: "GIT_COMMIT is : ${env.GIT_COMMIT}")
          slackSend (color: "add8e6", message: 'Mule4-Helloworld Deployment Started')
          buildsrc() 
          slackSend (color: "0000ff", message: 'Mule4-Helloworld Build Sucessfully')

      }
}

stage('upload to atifactory') {
    steps {
        script{
        sh "echo ${env.GIT_URL} > /tmp/giturl.txt"
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
            emailext attachLog: true, body: '''The Failed build details are as follows:<br> <br>
<table border="1">
<tr><td style="background-color:#FF0000;color:white">,<b>Job Name</b></td><td>$JOB_NAME</td></tr>
<tr><td style="background-color:#FF0000;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
<tr><td style="background-color:#FF0000;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
<tr><td style="background-color:#FF0000;color:white"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
</table>
''', subject: 'Mule 4 Cloud Deployment Status', to: 'devops@eaiesb.com'
           slackSend (color: "#FF0001",message: 'Mule4-Helloworld Deployment Failed')
        }
      success {
          emailext attachLog: true, mimeType: 'text/html', body: '''The jenkins build details are as follows:<br> <br>
<table border="1">
<tr><td style="background-color:#33339F;color:white">,<b>Job Name</b></td><td>$JOB_NAME</td></tr>
<tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
<tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
<tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
</table>
''', subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME} ${ENV, var="GIT_URL"}', to: 'devops@eaiesb.com'    
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
