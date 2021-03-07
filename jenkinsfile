def DOCKER_REGISTRY = "choisunguk"
def DEPLOYMNET_NAME = "springboot-demo"
def DEPLOYMENT_NAMESPACE = "default"

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
                    version = pom.version

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
                    sh "docker rmi ${docker_image_name}"
                }
            }
        }
        stage('get version'){
            steps{
                script{
                    image_name = sh(script:"""kubectl get po -A -o json | jq --raw-output '.items[].spec.containers[].image | select(. == "${docker_image_name}")' | sort | uniq""", returnStdout:true)
                    if(!image_name){
                        current_version = ''
                    }else{
                        current_version = image_name.split(":")[1].trim()
                    }
                }    
            }
        }
        stage('change deployment image name'){
            steps{
                dir ('kubernetes_resource'){
                    script {
                        def sed_docker_image_name = docker_image_name.replace("/", "\\/")
                        sh(script:"""sed -i "s/IMAGE_NAME/${sed_docker_image_name}/g" deployment.yaml""")
                    }
                }
            }
        }
        stage('deploy restart same image version'){
            when{
                expression{
                    current_version == version
                }
            }
            steps{
                dir ('kubernetes_resource'){
                    script {
                        sh(script: """kubectl apply -f .""")
                        sh(script: """kubectl rollout restart deployment ${DEPLOYMNET_NAME} -n ${DEPLOYMENT_NAMESPACE}""")
                    }
                }
            }
        }
        stage('deploy on kubernetes'){
            when{
                expression {
                    current_version != version
                }
            }
            steps{
                dir ('kubernetes_resource'){
                    sh 'kubectl apply -f .' 
                }
            }
        }
    }
}
