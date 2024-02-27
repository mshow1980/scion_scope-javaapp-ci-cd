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
                        def scannerHome = tool 'SonarQubeScanner3'
                        withSonarQubeEnv('SonarQube') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner
                        sonar.projectKey=scion_scope-javaapp-ci-cd
                        sonar.projectBaseDir=./scion_scope-javaapp-ci-cd
                        sonar.projectName=scion_scope-javaapp-ci-cd
                        sonar.projectVersion=1.0 
                        sonar.sources=./src/main/java 
                        sonar.language=java 
                        sonar.java.binaries=. 
                        sonar.scm.disabled=True 
                        sonar.sourceEncoding=UTF-8
                        """
                        }
                    }
                }
            }
        }
    }