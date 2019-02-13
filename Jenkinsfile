def yamlFile()
{
    echo 'start'
    def datas = readYaml file: '/var/lib/jenkins/jobs/YAML/workspace/propertyFile.yml'
    env.microservice = datas.microservice
    env.devproject = datas.devproject
    env.cicdproject = datas.cicdproject
    env.templatePath = datas.templatePath
    env.sonarTemplate = datas.sonarTemplate
}
def return1(name) 
{
    echo name
    openshift.withCluster() {
    openshift.withProject("${devproject}") {
    return openshift.selector('dc',"${microservice}").exists()
    }

}
}
def checout()
{
    checkout([$class: 'GitSCM', branches: [[name: '*/stable-ocp-3.10']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/lohitj/coolstore-microservice.git']]])
}

def BuildDecide(update)
{
    if(!update) {
        openshift.withCluster() {
	openshift.verbose()
        openshift.withProject("${devproject}") {
        openshift.newApp("${templatePath}") 
        }
    }
	BuildDecideSonar()
    }
    else  
    {
	    openshift.withCluster() {
	    openshift.withProject("${devproject}") {
        openshift.startBuild("${microservice}")
        }           	  
	    }
    }
}
def BuildDecideSonar()
{
    openshift.withCluster() {
	openshift.verbose()
    openshift.withProject("${cicdproject}") {
    openshift.newApp("${sonarTemplate}") 
        }
    }
}
node 
{
   
   
   def MAVEN_HOME = tool "MAVEN_HOME"
   def JAVA_HOME = tool "JAVA_HOME"
   env.PATH="${env.PATH}:${MAVEN_HOME}/bin:${JAVA_HOME}/bin"
    stage('Build')
   {
       checout()
       yamlFile()
       sh 'mvn -f cart-service/pom.xml clean compile'
   }
   stage('check')
   {
       BuildDecide(return1("${microservice}"))
   }
   stage('test')
   {
        sh 'mvn -f cart-service/pom.xml test'
   }
  
}
