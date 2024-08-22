pipeline {
    agent any
    
    environment {
        GIT_REPO = 'https://github.com/manishsalona/sampleMERNwithmicroservices.git'
        DOCKER_CRED_ID = 'dockerhub-login'
        DOCKERHUB_USERNAME = 'salona1993'
        DOCKERHUB_REPO = 'sample-mern-microservice'
        DOCKERFILE_HELLO = 'backend/helloService'
        DOCKERFILE_PROFILE = 'backend/profileService'
        DOCKERFILE_FRONTEND = 'frontend'
    }

    stages {
        stage('Cloning the source code') {
            steps {
                git branch: 'main', url: "${env.GIT_REPO}"
            }
        }

        stage('Building Docker Image of helloService') {
            agent { label 'Jenkins-Slave' }
            steps {
                echo 'Running on the slave node...'
                script {
                    docker.build("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:hello-service-${env.BUILD_ID}", "${env.DOCKERFILE_HELLO}")
                }
            }
        }

        stage('Building Docker Image of profileService') {
            steps {
                script {
                    docker.build("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:profile-service-${env.BUILD_ID}", "${env.DOCKERFILE_PROFILE}")
                }
            }
        }

        stage('Building Docker Image of Frontend') {
            steps {
                script {
                    docker.build("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:frontend-${env.BUILD_ID}", "${env.DOCKERFILE_FRONTEND}")
                }
            }
        }

        stage('Push the Image to DockerHub Repository') {
            steps {
                script {
                    docker.withRegistry('', "${env.DOCKER_CRED_ID}") {
                        docker.image("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:hello-service-${env.BUILD_ID}").push()
                        docker.image("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:profile-service-${env.BUILD_ID}").push()
                        docker.image("${env.DOCKERHUB_USERNAME}/${env.DOCKERHUB_REPO}:frontend-${env.BUILD_ID}").push()
                    }
                }
            }
        }
    }
}
