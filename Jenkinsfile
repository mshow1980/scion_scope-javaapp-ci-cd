pipeline{
    
    agent any

    stages{
        stage('Clean Workspace'){

            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('checkout workspace'){
            steps{
                script{
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], 
                    userRemoteConfigs: [[url: 'https://github.com/mshow1980/scion_scope-javaapp-ci-cd.git']])
                }
            }
        }
    }

}