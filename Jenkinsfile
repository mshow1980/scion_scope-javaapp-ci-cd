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
        stage('OWASP Dependency-Check Vulnerabilities'){
            steps{
                script{
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                    }
                }
            }
            stage('sonarqube scan'){
                steps{
                    script{
                        withSonarQubeEnv(credentialsId: 'Jenkins-Token') {
                        mvn clean package sonar:sonar -Dsonar.login=Jenkins-Token
                        }
                        }
                    }
                }
            }
        }