node
{
  def mavenHome = tool name: "maven3.8.4"
stage('Checkout Code')
{
git branch: 'development', credentialsId: 'd19921f2-c700-4d80-a951-3a164f347a41', url: 'https://github.com/mss-ec-ap/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployIntoTomcat')
{
    sshagent(['6fd46a48-d3a9-4444-a120-74bd2625e1dc']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.83.17:/opt/apache-tomcat-9.0.56/webapps/"
}
}

stage('SendNotifications')
{
    emailext body: '''Build Over
Regards,
Amruta''', subject: 'Biuld Over', to: 'desh.ammu@gmail.com'
    }
}
