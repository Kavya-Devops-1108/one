pipeline {
   agent any 
tools {
  maven 'mymaven'
}
stages {
    stage ("Code"){
       steps {
            git 'https://github.com/Kavya-Devops-1108/one.git'
            }
         }
    stage("Build"){ 
           steps {
                  sh 'mvn install'
                }
            }
stage ("nexus" ) {
steps {
  nexusArtifactUploader artifacts: [[artifactId: '${artifactid}', classifier: '', file: 'target/${artifactid}-${version}.war', type: 'war']], credentialsId: 'nexus', groupId: '${groupid}', nexusUrl: '3.93.47.84:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'myrepo', version: '${version}'
}
}
    stage("Package"){
             steps {
                   deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat', path: '', url: 'http://54.196.105.89:8080/')], contextPath: 'myapp', war: 'target/*.war'   
               }
              }
}
}
