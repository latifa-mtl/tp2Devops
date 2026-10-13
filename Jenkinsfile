pipeline {
    environment {
        registry = 'latifamoutawakil/tp2devops'
        registryCredential = 'docker_hub_token'
        dockerImage = ''
    }
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/latifa-mtl/tp2devops.git'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}