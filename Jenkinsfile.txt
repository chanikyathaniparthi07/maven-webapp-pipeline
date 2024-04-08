pipeline {
    agent any

    environment {
        
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch:'main', url: 'https://github.com/chanikyathaniparthi07/maven-web-app-jenkins.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('Sonar-Server-7.6') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
}
