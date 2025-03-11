node{
    def mavenHome = tool name: "maven3.9.9"
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
