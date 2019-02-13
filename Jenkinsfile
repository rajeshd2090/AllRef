def checout(){
      checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'e35ac079-efc0-4391-8d43-9d4c95371ae3', url: "${GIT_URL}"]]])
}
pipeline {
    agent any
    environment {
        GIT_URL='git@infygit.ad.infosys.com:lohit.jain'
    }
    tools{
        maven 'Maven_HOME'
        jdk   'JAVA_HOME'
    }
 stages {
        stage ('testconvert - Checkout') {
            steps{
                
                checout()
            }
 	  
        }
        stage('Test') {
            steps {
                bat 'mvn test'
                junit '**/target/surefire-reports/*.xml'
            }
        }
        stage('Packaging') {
            steps {
                bat 'mvn war:war' 
            }
        }
        stage('Deploy') {
            steps {
                bat 'mvn deploy' 
            }
        }
        
}
post {
        always {
            echo "I AM ALWAYS first"
        }
        aborted {
            echo "BUILD ABORTED"
        }
        success {
            echo "BUILD SUCCESS"
        }
        unstable {
            echo "BUILD UNSTABLE"
        }
        failure {
            echo "BUILD FAILURE"
        }
    }
}
