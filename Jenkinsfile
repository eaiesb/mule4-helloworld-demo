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
stage('renaming the target zip file') {
    steps {
               sh 'mv target/helloworld-1.0.0-SNAPSHOT-mule-application.jar helloworld.jar'
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

