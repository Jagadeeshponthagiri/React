pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = 'myreact'
        DOCKER_HUB_CREDENTIALS = 'dockerhub_id'
        DOCKER_HUB_REPO = 'imr99/myreact'  
        CONTAINER_NAME = 'myreactappContainer'
    }
    
    stages {
        stage('Clone Git Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/imr99/React.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE_NAME} ."
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: "${DOCKER_HUB_CREDENTIALS}", url: '') {
                        sh "docker tag ${DOCKER_IMAGE_NAME} ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                        sh "docker push ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                    }
                }
            }
        }
        
        stage('Docker Run') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    
                    //def dockerImage = docker.image("${DOCKER_HUB_REPO}:${BUILD_NUMBER}")
                    sh "docker container run -itd --name ${CONTAINER_NAME} -p 3000:3000 ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                }
            }
        }
    }
}