pipeline {
    agent {
        node {
            label "t059-runner"
        }
    }
    tools {
        maven 'maven'
    }
    stages {
        stage('Build Maven') {
            steps {
               checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'ca-git-access', url: 'https://git.cloudavise.com/visops/t059/jenkins-pipeline.git']])
               
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh "sudo docker build -t jenkins-1 ."
                }
            }
        }
        stage('Push image to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-t059', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {

                        sh "echo ${DOCKER_HUB_PASSWORD} |sudo docker login --username ${DOCKER_HUB_USERNAME} --password-stdin"
                        sh "sudo docker tag jenkins-1 srinu9666/jenkins-1"
                        sh "sudo docker push srinu9666/jenkins-1"
                    }
                }
            }
        }
    }
    
}

