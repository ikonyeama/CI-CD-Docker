import java.text.SimpleDateFormat                                                                       
                                                                                                        
pipeline {                                                                                              
    agent any                                                                                           
       stages {                                                                                         
        stage("initialize") {                                                                           
            steps {                                                                                     
              script {                                                                                  
                def dateFormat = new SimpleDateFormat("yy.MM.dd")                                       
                currentBuild.displayName = dateFormat.format(new Date()) + "-" + env.BUILD_NUMBER       
                def dockerHome = tool 'myDocker'                                                        
                def mavenHome  = tool 'myMaven'                                                         
                env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"                             
               }                                                                                        
              withCredentials([usernamePassword(                                                        
                credentialsId: "dockerHubAccount",                                                      
                usernameVariable: "dockerUser",                                                           
                passwordVariable: "dockerPassword",                                                           
              )]) {                                                                                     
                sh "docker login -u $dockerUser -p $dockerPassword"                                             
              }                                                                                         
            }                                                                                           
          }                                                                                             
        stage('jenkins') {                                                                              
            steps {                                                                                     
                sh "cd jenkins && docker image build --no-cache -t nginx ."                             
                sh "docker image tag nginx:${currentBuild.displayName} $dockerUser/jenkins-pipeline:latest"                           
                sh "docker image push $dockerUser/jenkins-pipeline:latest"                                
            }                                                                                           
        }                                                                                               
    }                                                                                                   
}         
