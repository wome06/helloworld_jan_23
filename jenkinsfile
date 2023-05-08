pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = ''
    registryCredential = 'jenkins-ecr'
    dockerimage = '664965757957.dkr.ecr.us-east-1.amazonaws.com/devop_repository'
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/wome06/helloworld_jan_23.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}
