pipeline {
  agent any

  stages {

    stage ('Build docker image')
      steps { 
        script {
            dockerapp = docker.build("rodrigordavila/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
            
        }
      }


  }

}