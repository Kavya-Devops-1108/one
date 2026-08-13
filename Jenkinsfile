pipeline {
    agent {
        node {
            label 'test'
        }
    }
    tools {
        maven 'maven01'
    }
   stages {
       stage("cleanws"){
           steps {
               cleanWs()
           }
       }
       stage("Code Checkout") {
           steps {
              git 'https://github.com/Kavya-Devops-1108/one.git'
           }
       }
       stage("build"){
           steps {
               sh 'mvn clean package'
           }
       }
       stage("CQA"){
           steps {
               withSonarQubeEnv("sonar") {
                  sh "mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=Myproj"
              }
           }
       }
       stage("Qualitygates"){
           steps {
               waitForQualityGate abortPipeline: true, credentialsId: 'sonar'
           }
       }
       stage("Nexus Artifact") {
           steps {
               nexusArtifactUploader artifacts: [[artifactId: 'myweb', classifier: '', file: 'target/myweb-8.6.9.war', type: 'war']], credentialsId: 'nexus', groupId: 'in.javahome', nexusUrl: '54.196.141.30:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'myrepos', version: '8.6.9'
              }
       }
       stage("Docker images"){
           steps {
               sh 'docker build -t myimg .'
               sh 'docker tag myimg kavya318506/myjavaimg'
           }
       }
       stage("trivy"){
           steps {
               sh 'trivy image kavya318506/myjavaimg '
           }
       }
       stage("push to registry"){
           steps {
               withDockerRegistry(credentialsId: 'dockerhub') {
                 sh 'docker push kavya318506/myjavaimg'
                }
           }
       }
       stage("deploy"){
           steps {
               deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat', path: '', url: 'http://98.93.105.233:8080/')], contextPath: 'myapplication', war: 'target/*.war'
           }
       }
   }
    post {
        success {
        emailext(
            to: 'kavyachadalavada1108@gmail.com',
            subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """
Hello Team,

Jenkins Pipeline Status: ${currentBuild.currentResult}

Job Name     : ${env.JOB_NAME}
Build Number : ${env.BUILD_NUMBER}
Build Status : ${currentBuild.currentResult}
Build URL    : ${env.BUILD_URL}

Regards,
Jenkins
"""
        )
    }
   }
    }
