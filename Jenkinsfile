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
        stage('build image and push to dockerHub') {
            steps {
                sh "cd nginx && docker image build --no-cache -t nginx:latest ."
                sh "docker image tag nginx:latest $dockerUser/nginx-pipeline:latest"
                sh "docker image push $dockerUser/nginx-pipeline:latest"
            }
        }
		stage('pull image and run image on container') {
            steps {
			    sh "docker pull $dockerUser/nginx-pipeline:latest"
                sh "docker run -d -rm -p 8090:8090 --name nginx-pipeline $dockerUser/nginx-pipeline:latest"
                echo "Application started on port: 8090 using http"
            }
        }
    }
}
