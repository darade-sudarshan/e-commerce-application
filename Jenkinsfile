pipeline {
    environment {
        imagename = "daradesudarshan/centralrepo"
        frontendDockerImage = ''
        backendDockerImage = ''
        dockerHubCredentials = 'docker'
        kubernetes_server_private_ip="192.168.49.2"
        ansible_server_private_ip="localhost"
        KUBECONFIG_CREDENTIALS_ID = 'my-kubernetes' // Update with your Kubeconfig credentials ID
        KUBECONFIG_FILE = 'kubeconfig'
    }
 
    agent any
 
    stages {
        stage('Cloning Git') {
            steps {
                git([url: 'https://github.com/darade-sudarshan/e-commerce-application.git', branch: 'main'])
            }
        }
 
        stage('Building frontend image') {
            steps {
                script {
                    dir('frontend') {
                        frontendDockerImage = docker.build "${imagename}:node-app-frontend"
                    }
                }
            }
        }
         stage('Building backend image') {
            steps {
                script {
                    dir('backend') {
                        backendDockerImage = docker.build "${imagename}:node-app-backend"
                    }
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
                script {
                    // Use Jenkins credentials for Docker Hub login
                    withCredentials([usernamePassword(credentialsId: dockerHubCredentials, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
 
                        // Push the image
                        sh "docker push ${imagename}:node-app-frontend"
                        sh "docker push ${imagename}:node-app-frontend"
                    }
                }
            }
        }
        stage('Setup Kubernetes Context') {
            steps {
                script {
                    withCredentials([file(credentialsId: "${KUBECONFIG_CREDENTIALS_ID}", variable: 'KUBECONFIG_FILE')]) {
                        sh 'mkdir -p $HOME/.kube'
                        sh 'cp $KUBECONFIG_FILE $HOME/.kube/config'
                        sh 'chmod 600 $HOME/.kube/config'
                    }
                }
            }
        }
        stage('Test minikube Connection') {
            steps {
                withCredentials([ string(credentialsId: 'my-kubernetes')]) {
                    sh 'kubectl --server https://192.168.49.2:8443   get all -n production -o wide '
                    }
                }
            }    
    }
        
    // stage('Copy files from jenkins to kubernetes server'){
     
    // }
 
    // stage('Kubernetes deployment using ansible'){
    
    // }
     // stage('webapp testing'){
    
    // }
    
}