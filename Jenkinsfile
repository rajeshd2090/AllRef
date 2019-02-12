def return1(name) 
{
    openshift.withCluster() {
    openshift.withProject('VARIABLE_NAME') {
    return openshift.selector('dc', name).exists()
    }

}
}
def checout()
{
    checkout([$class: 'GitSCM', branches: [[name: '*/VARIABLE_NAME']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: "${GIT_URL}"]]])
}

def BuildDecide(update)
{
    if(!update) {
        openshift.withCluster() {
	openshift.verbose()
        openshift.withProject('VARIABLE_NAME') {
        openshift.newApp("${templatePath}") 
        }
    }
	BuildDecideSonar()
    }
    else  
    {
	    openshift.withCluster() {
	    openshift.withProject('VARIABLE_NAME') {
        openshift.startBuild("web-ui")
        }           	  
	    }
    }
}
def BuildDecideSonar()
{
    openshift.withCluster() {
	openshift.verbose()
    openshift.withProject('VARIABLE_NAME') {
    openshift.newApp("${sonarQube}") 
        }
    }
}

pipeline 
{
    agent any
    environment 
    {
        GIT_URL='VARIABLE_NAME'
	    templatePath = 'VARIABLE_NAME'
        sonarQube = 'VARIABLE_NAME'
    }
    tools{
        maven 'MAVEN_HOME'
        jdk   'JAVA_HOME'
    }

  stages 
  {
        stage ('check') 
        {
	        steps
            	{
                	BuildDecide(return1('VARIABLE_NAME'))
	        }
        }
        stage ('Build') 
        {
            steps
            {
                checout()
                sh 'mvn -f cart-service/pom.xml clean integration-test'
            }
        }
	stage ('Sonar')
        {
            steps
            {
                sh "mvn -f cart-service/pom.xml sonar:sonar -Dsonar.host.url='http://sonar-coolstore-dev-lohit.apps.na39.openshift.opentlc.com' -X"

            }
	    
        }
	stage ('Unit test')
	  {
		  steps
		  {
			sh "mvn -f cart-service/pom.xml test"
		  }
	  }
	
        
	}
    
}
