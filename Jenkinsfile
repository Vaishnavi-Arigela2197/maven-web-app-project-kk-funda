node{

    echo "git job name: ${env.JOB_NAME}"
    echo "build number is: ${env.BUILD_NUMBER}"
    echo "node name is: ${env.NODE_NAME}"
    echo "branch name is: ${env.BRANCH_NAME}"
    
    def mavenHome = tool name: "maven3.9.9"
    try{
       notifyBuild('STARTED') 
    stage('Git checkout')
    {
        git branch: 'development', credentialsId: 'd6ab28bc-cd75-4ada-bce1-befbda9bc16a', url: 'https://github.com/Vaishnavi-Arigela2197/maven-web-app-project-kk-funda.git'
    }
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('Sonarqube report')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('Nexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('Tomcat deploy')
    {
        echo "Deploying war file using url"
        sh """
        curl -u vaish:vaish \
        --upload-file /var/lib/jenkins/workspace/first-pipeline/target/maven-web-application.war \
        "http://13.235.248.96:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
    }  
    }
    catch (e) {
        currentBuild.result = "FAILED"
        throw e
    }
    finally {
        notifyBuild(currentBuild.result)
    }
}
def notifyBuild(string buildStatus = 'STARTED')
buildStatus = buildStatus ?: 'SUCCESS'

def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}] [${env.BRANCH_NAME}]'"
  def summary = "${subject} (${env.BUILD_URL})" 

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}




