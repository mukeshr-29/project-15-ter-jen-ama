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
        stage('dependency check'){
            steps{
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('file scan'){
            steps{
                sh 'trivy fs . > TRIVYFS.txt'
            }
        }
        stage('docker build & push'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        sh '''
                        docker build -t amazon-project .
                        docker tag amazon-project mukeshr29/amazon-project
                        docker push mukeshr29/amazon-project
                        '''
                    }
                }
            }
        }
        stage('trivy img scan'){
            steps{
                sh 'trivy image mukeshr29/amazon-project'
            }
        }
    }
}