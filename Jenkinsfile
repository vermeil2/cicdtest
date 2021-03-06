def DOCKER_REGISTRY = "choisunguk/demo-springboot"
def TAG = "dev"
def DOCKER_IMAGE_NAME = "${DOCKER_REGISTRY}/${JOB_BASE_NAME}:${TAG}"

pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('build and push dockerimage'){
            steps{
                def builded_dockerimage = docker.build("${DOCKER_IMAGE_NAME}")
            }
            steps{
                builded_dockerimage.push()
            }
        }
    }
}
