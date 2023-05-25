pipeline {
    agent { 
        node {
            label "t059-runner"
        }
    } 
    tools {
        maven 'maven"
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
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh "docker login -u srinu9666 -p ${dockerhubpwd}"
                        
                        sh 'docker tag jenkins/devops-integration srinu-jenkins/jenkins-docker-image:v1'
                        
                        sh 'docker push srinu9666/srinu-jenkins:v1' 
                    }
                }
            }
        }
    }
}
