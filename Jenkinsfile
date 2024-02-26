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
        stage('clean install'){
            steps{
                script{
                    sh 'mvn clean package'
                }
            }
        }
        stage('sonar analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'SOnar-Token') {
                        mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=scion_scope-javaapp-ci-cd \
                        -Dsonar.host.url=http://44.221.61.24:9000 \
                        -Dsonar.login=SOnar-Token
                    }
                }
            }
        }
    }
}
