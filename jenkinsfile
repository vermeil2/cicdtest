def DOCKER_REGISTRY = ""
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
        stage('docker build'){
            steps{
                sh "docker build -t ${DOCKER_IMAGE_NAME} ."                
            }
        }
    }
}
