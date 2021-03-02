pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://gitlab.com/springboot-reactjs/springboot.git']]]
            }
        }
    }
}
