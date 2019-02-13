pipeline {
    agent any
    tools {
        maven 'Maven_HOME'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/piomin/sample-spring-boot-autoscaler.git', credentialsId: 'github-piomin', branch: 'master'
            }
        }
        stage('Test') {
            steps {
                dir('example-service') {
                    sh 'mvn clean test'
                }
            }
        }
        stage('Build') {
            steps {
                dir('example-service') {
                    sh 'mvn clean install'
                }
            }
        }
    }
    post {
        always {
            junit '**/target/reports/**/*.xml'
        }
    }
}
