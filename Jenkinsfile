pipeline {
    agent any
    
    tools{
        maven 'maven-iti'
        jdk 'JDK17'
    }

    
    stages{
        stage('clone git repo'){
            steps{
              git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic.git'
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
        stage('run .jar'){
            steps{
              sh 'nohup java -jar target/*.jar >app.log 2>&1 &'
              sh 'sleep 60'
            }
        }
    }
}
