pipeline{
    agent any 
    environment{
        SCANNER_HOME= tool 'SonarQube-Scanner'
    }
    
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
                    withSonarQubeEnv('sonar:sonar') {
                        sh """
                        $SCANNER_HOME/bin/SonarQube-Scanner 
                        -Dsonar.projectKey=scion_scope-javaapp-ci-cd 
                        -Dsonar.java.binaries=. / 
                        """
                    }
                }
            }
        }
    }
}
