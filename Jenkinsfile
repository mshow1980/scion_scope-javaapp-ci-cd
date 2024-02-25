pipeline{
    agent {
        docker {
            image 'maven'
        }
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
        stage('sonar quality check'){
            steps{
            environment {
                SCANNER_HOME = tool 'sonar-scanner'
            }
                script{
                    withSonarQubeEnv(credentialsId: 'SOnar-Token') {
                        sh "${SCANNER_HOME}/bin/sonar-scanner \
                            -Dsonar.projectKey=scion_scope-javaapp-ci-cd \
                            -Dsonar.sources=. "
                        sh "mvn clean package sonar:sonar"
                    }
                }
            }
        }
    }
}
