pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building..'
                script {
                    dockerapp = docker.build("rodrigordavila/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage('Registry imagem') {
            steps {
                echo 'Dockerhub registry'
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockehub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
              
    }
}