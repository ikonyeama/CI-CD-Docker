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
                usernameVariable: "USERNAME",
                passwordVariable: "PASSWORD",
              )]) {
                sh "docker login -u $USERNAME -p $PASSWORD"
              }
            }
          }
        stage('jenkins') {
            steps {
                sh "cd jenkins && docker image build --no-cache -t nginx ."
                sh "docker image tag nginx nginx:${currentBuild.displayName}"
                sh "docker image push nginx:${currentBuild.displayName}"
                sh "docker image push nginx"
            }
        }
    }
}
