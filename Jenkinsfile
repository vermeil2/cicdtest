pipeline {
    agent any
    tools {
        maven 'maven_3.9.9'
    }
    stages {
        stage('build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('docker build and push') {
            steps {
                script {
                        docker.withRegistry('https://registry.hub.docker.com', 'publicdocker'){
                        image = docker.build("pymyp131/cicdtest:v2")
                        image.push()
                    }
                }
            }
        }
    }
}
