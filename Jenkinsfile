pipeline {
    agent any
    
    tools{
        maven 'maven-iti'
        jdk 'JDK17'
    }

    
    stages{
        stage('clone git repo'){
            steps{
              git branch: 'main', url: 'https://github.com/Nada-Abdelghany/spring-petclinic.git'
            }
        }
        stage('change exposed port'){
            steps{
              sh 'echo server.port=9090 >> src/main/resources/application.properties'
            }
        }
        stage('compile code'){
            steps{
              sh 'mvn clean compile'
            }
        }
        stage('test code'){
            steps{
              sh 'mvn test'
            }
        }
        stage('package'){
            steps{
              sh 'mvn package'
            }
        }
        stage('build docker image'){
            steps{
              sh 'docker build -t petclinic:latest -t petclinic:${BUILD_NUMBER} .'
            }
        }
        stage('push docker image'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                sh "echo $PASS | docker login -u $USER --password-stdin"
                }
                sh 'docker push nada410/petclinic:latest'
                sh 'docker push nada410/petclinic:${BUILD_NUMBER}'
            }
        }
        stage('run container'){
            steps{
              sh 'docker run -dp 9090:9090 petclinic:latest '
            }
        }
    }
}
