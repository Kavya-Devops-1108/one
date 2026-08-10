pipeline {
   agent {
      node {
         label 'test'
      }
   }
   tools {
      maven 'mymaven' 
   }
      stages {
         stage("cleanws") {
            steps {
                cleanWs()
            }
         }
          stage("code") {
               steps {
                   git "https://github.com/Kavya-Devops-1108/one.git"
               }
          }
         stage("Build") {
            steps {
               sh 'mvn clean package' 
            }
         }
         stage("cQA"){
            steps {
               withSonarQubeEnv("sonar") {
               sh "mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=project"
                      } 
                }
           }
         stage("Quality gate") {
            steps {
               waitForQualityGate abortPipeline: true, credentialsId: 'sonarqube'
            }
         }
         stage("docker Image") {
            steps {
                sh 'docker build -t myimage .'
            }
         }
         stage("Image Scan") {
            steps {
                sh 'trivy image myimage'
            }
         }
         stage("REgistry") {
            steps {
               sh 'docker tag myimage kavya318506/practice'
               sh 'docker push kavya318506/practice'
            }
         }
      }
   }
         
