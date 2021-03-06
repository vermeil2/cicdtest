def DOCKER_REGISTRY = "choisunguk/demo-springboot"
def TAG = "dev"
def DOCKER_IMAGE_NAME = "${DOCKER_REGISTRY}:${TAG}"

pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('build docker image'){
            steps{
                script{
                    builded_dockerimage = docker.build("${DOCKER_IMAGE_NAME}")
                }
            }
        }
        stage("push dockerimage"){
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        builded_dockerimage.push()
                    }
                }
            }
        }
    }
}
