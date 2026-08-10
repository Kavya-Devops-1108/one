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
         stage"cleanws") {
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
      }
   }
         
