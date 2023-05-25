pipeline {
    agent { 
        node {
            label "t059-runne"
        }
    } 
    tools {
        maven 'maven'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://git.cloudavise.com/visops/t059/jenkins-pipeline.git']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t jenkins/devops-integration .'
                }
            }
        }
        stage('Push image to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-sr', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        
                        sh "echo ${DOCKER_HUB_PASSWORD} |sudo docker login --username ${DOCKER_HUB_USERNAME} --password-stdin"
                        
                        sh 'docker tag jenkins/devops-integration srinu-jenkins/jenkins-docker-image:v1'
                        
                        sh 'docker push srinu9666/srinu-jenkins:v1' 
                    }
                }
            }
        }
    }
}
