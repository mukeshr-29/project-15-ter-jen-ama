pipeline{
    agent any
    tools{
        jdk 'jdk17'
        node 'node16'
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
    }
}