node{

def mavenHome = tool name: "maven3.9.1"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', 
daysToKeepStr: '1', numToKeepStr: '3')), [$class: 'JobLocalConfiguration', changeReasonComment: ''],
pipelineTriggers([pollSCM('* * * * * ')])])

stage("checkout"){
git credentialsId: '2713204d-d343-4969-bd23-73c80b106f8d', url: 'https://github.com/NeeravN/maven-web-application.git'
}
stage("build maven"){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}


stage('DeployAppIntoTomcatserver'){
sshagent(['08f9dbf2-0dc8-47c8-983a-a24433db7b01']) {
   sh "scp -o StrictHostkeychecking=no target/maven-web-application.war ec2-user@172.31.44.14:/opt/apache-tomcat-9.0.75/webapps/" 
}
}

}
