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
                checkout(
                    scm: [
                        $class: 'GitSCM',
                        branches: [[name: '*/master']],
                        userRemoteConfigs: [[url: 'https://git.cloudavise.com/visops/t059/jenkins-pipeline.git']]
                    ]
                )
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
                    withCredentials([
                        usernamePassword(credentialsId: 'dockerhub-sr', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')
                    ]) {
                        sh "echo \${DOCKER_HUB_PASSWORD} | sudo docker login --username \${DOCKER_HUB_USERNAME} --password-stdin"
                        sh "sudo docker tag jenkins/devops-integration:\${BUILD_NUMBER} \${DOCKER_HUB_USERNAME}/\${DOCKER_REPO_NAME}:\${BUILD_NUMBER}"
                        sh "sudo docker push \${DOCKER_HUB_USERNAME}/\${DOCKER_REPO_NAME}:\${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}

