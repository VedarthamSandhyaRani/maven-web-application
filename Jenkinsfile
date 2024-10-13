node{
def mavenHome = tool name: 'maven3.9.8'

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([cron('* * * * *'), pollSCM('')])])
echo "job name is ${env.JOB_NAME}"
echo "Build number is : ${env.BUILD_NUMBER}"
echo "The node name is : ${env.NODE_NAME}"
echo "The job url is :${env.JOB_URL}"


stage('CheckoutCode')
{
git branch: 'dev', credentialsId: '8190a80d-50f0-4780-9e89-13555c968647', url: 'https://github.com/VedarthamSandhyaRani/maven-web-application.git'
}

stage('Build'){
sh "$mavenHome/bin/mvn clean package"
}

stage('UploadArtifactsIntoNexus'){
sh "$mavenHome/bin/mvn clean deploy"
}


stage('Deploy application tomcat'){
sshagent(['9dd64893-fff3-4bba-8e8c-8a25d3cd3c23']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.11.128:/opt/apache-tomcat-9.0.95/webapps/"
}
}

}


