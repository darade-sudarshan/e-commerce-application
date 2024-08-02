pipeline {
    environment {
        imagename = "daradesudarshan/centralrepo"
        frontendDockerImage = ''
        backendDockerImage = ''
        dockerHubCredentials = 'docker'
        kubernetes_server_private_ip="192.168.49.2"
        ansible_server_private_ip="localhost"
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
    }
    // stage('Sending Dockerfile to Ansible'){
       
    // }
    // stage('Copy files from jenkins to kubernetes server'){
     
    // }
 
    // stage('Kubernetes deployment using ansible'){
    
    // }
}