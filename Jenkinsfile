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
                    docker.withRegistry('https://registry.hub.docker.com', dockerub)
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                }
            }
        }
        stage('Deploy K8S') {
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                echo 'Deploying....'
                withKubeconfig([credentialsID: 'kubeconfig']) {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployments.yaml'
                    sh 'kubectl apply -f ./k8s/deployments.yaml'
                }

            }
        }
    }
}
