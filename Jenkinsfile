pipeline {
    agent any

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
                        image = docker.build("choisunguk/demo-springboot:v2")
                        image.push()
                    }
                }
            }
        }
    }
}
