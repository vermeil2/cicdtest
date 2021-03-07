def DOCKER_REGISTRY = "choisunguk"

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
                    def pom = readMavenPom file: 'pom.xml'
                    def version = pom.version

                    docker_image_name = "${DOCKER_REGISTRY}/demo-springboot:${version}"
                    builded_dockerimage = docker.build("${docker_image_name}")
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
        stage('deploy on kubernetes'){
            steps{
                echo docker_image_name
                dir ('kubernetes_resource'){
                    sh 'sed -i "s/IMAGE_NAME/${docker_image_name}/g" deployment.yaml'
                    sh 'kubectl apply -f .' 
                }
            }
        }
    }
}
