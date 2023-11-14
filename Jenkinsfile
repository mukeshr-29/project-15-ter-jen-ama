pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment{
        SCANNER_HOME=tool 'sonar_scanner'
    }
    stages{
        stage('clean work space'){
            steps{
                cleanWs()
            }
        }
        stage('git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/mukeshr-29/project-15-ter-jen-ama.git'
            }
        }
        stage('sonar analysis'){
            steps{
                script{
                    withSonarQubeEnv('sonarqube'){
                        sh '''
                        $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=amazon \
                            -Dsonar.projectKey=amazon 
                        '''
                    }
                }
            }
        }
        stage('quality gate check'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube'
                }
            }
        }
        stage('install npm'){
            steps{
                sh 'npm install'
            }
        }
    }
}