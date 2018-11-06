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
steps { buildsrc() }
}
stage('renaming the target zip file') {
    steps {
               sh 'mv target/helloworld-1.0.0-SNAPSHOT-mule-application.jar helloworld.jar'
    }
}  	
}
}
// steps
def buildsrc() {
dir ('.' ) {
    sh '/usr/maven/apache-maven-3.3.9/bin/mvn clean package deploy -DmuleDeploy'
}
}
