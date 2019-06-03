pipeline {
   environment {
    def CONTAINER_NAME=”jenkins-pipeline”
    def CONTAINER_TAG=”latest”
    def DOCKER_HUB_USER=”ikonyeama"
    def HTTP_PORT=”8090"
	def REGISTRY_ADDRESS="ikonyeama"
    }
 agent any
    stages {
stage('Initialize'){
    def dockerHome = tool 'myDocker'
    def mavenHome  = tool 'myMaven'
    env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
}
stage('Push to Docker Registry'){
    withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        pushToImage(CONTAINER_NAME, CONTAINER_TAG, USERNAME, PASSWORD)
    }
}

def pushToImage(containerName, tag, dockerUser, dockerPassword){
   sh “docker login -u $dockerUser -p $dockerPassword”
     sh “docker tag $containerName:$tag $dockerUser/$containerName:$tag”
     sh “docker push $dockerUser/$containerName:$tag”
     echo “Image push complete”
    }

    def runApp(containerName, tag, dockerHubUser, httpPort){
     sh “docker pull $dockerHubUser/$containerName”
     sh “docker run -d — rm -p $httpPort:$httpPort — name $containerName $dockerHubUser/$containerName:$tag”
     echo “Application started on port: ${httpPort} (http)”
    }
   }
}
