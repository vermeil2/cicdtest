pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                sh "mvn clean package"
                echo "test1234512321312"
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
