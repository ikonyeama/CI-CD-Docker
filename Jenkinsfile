import java.text.SimpleDateFormat

pipeline {
    agent any
    tools {
	docker 'myDocker'
        maven 'myMaven'
    }
    stages {
	stage('test docker installation') {
	    steps {
		sh 'docker version'
		}
	}
        stage('init') {
            steps {
              script {
                def dateFormat = new SimpleDateFormat("yy.MM.dd")
                currentBuild.displayName = dateFormat.format(new Date()) + "-" + env.BUILD_NUMBER
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
