pipeline {
    agent {
        docker {
            image 'maven'
        }
    }
    stages {
        stage ('Clean Workspace') {
            steps {
                script {
                    cleanWs()
                }
            }
        }
    }
}