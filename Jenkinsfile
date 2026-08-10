pipeline {
agent any
tools {
   maven "mymaven'
  }
stages {
   stage("Code") {
          steps {
                 sh 'https://github.com/Kavya-Devops-1108/one.git'
                 }
              }
   stage("Build") {
                steps {
                       sh 'mvn clean install'
                             }
                    }
   stage ("CQA") {
                  steps {
                         withSonarQubeEnv("mysona") {
                            sh "mvn clean verify sonar:sonar -Dsonar.projectKey=myproj"
                               }

                 }
}
  stage("quality gate") {
        steps {
                waitForQualityGate abortPipeline: true, credentialsId: 'sonarqube'
              }
    }
}
}
