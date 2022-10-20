pipeline {
    agent any
    
    stages {

        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("darosas76/kube-news:${env.BUILD_ID}", '-f ./src/dockerfile ./src')
                }
            }
        }
       
        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                        }
                }

            }
        }

        stage ('Deploy Kubernetes'){
            steps {
                whithKubeConfig ([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f ./k8s/deplyment.yaml'

                }
            }
        }
    }
}

