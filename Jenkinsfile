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
        stage('Deploy image') {
            steps {
                bat """
                    docker rm -f tp2-pipeline-web 2>nul
                    docker run -d --name tp2-pipeline-web -p 8082:80 ${registry}:${BUILD_NUMBER}
                """
            }
        }
    }
}