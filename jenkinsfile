pipeline {
    agent any
    
    environment {
        JAVA_HOME = "/opt/java/openjdk/bin"
    }

    stages {
        stage('build') {
            steps {
                sh "mvn clean package"
            }
        }
    }
}
