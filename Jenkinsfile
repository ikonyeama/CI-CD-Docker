pipeline {                                                                                              
    agent any                                                                                           
       stages {                                                                                         
        stage("initialize") {                                                                           
            steps {                                                                                     
              script {                                                                                  
                def dockerHome = tool 'myDocker'                                                        
                def mavenHome  = tool 'myMaven'                                                         
                env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"                             
               }                                                                                        
              withCredentials([usernamePassword(                                                        
                credentialsId: "dockerHubAccount",                                                      
                usernameVariable: "USERNAME",                                                           
                passwordVariable: "PASSWORD",                                                           
              )]) {                                                                                     
                sh "docker login -u $dockerUser -p $dockerPassword"                                             
              }                                                                                         
            }                                                                                           
          }                                                                                             
        stage('jenkins') {                                                                              
            steps {                                                                                     
                sh "cd nginx && docker image build --no-cache -t nginx:latest ."                             
                sh "docker image tag nginx:latest $dockerUser/jenkins-pipeline:latest"                           
                sh "docker image push $dockerUser/jenkins-pipeline:latest"                                
            }                                                                                           
        }                                                                                               
    }                                                                                                   
}         
