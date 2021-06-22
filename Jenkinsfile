node {

def mavenHome = tool name: "maven3.6.3"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])

stage('CheckoutCode'){
git branch: 'development', credentialsId: '4337811d-0368-4314-a858-809d021f874b', url: 'https://github.com/srinivastrainer/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}

stage('DepoyAppIntoTomcatServer'){
sshagent(['23fd4af2-7196-4283-8f73-c014ee907e33']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.58.73.109:/opt/apache-tomcat-9.0.48/webapps/"   
}
}

stage('SendEmailNotification'){
emailext body: 'Build Completed.', subject: 'Build Completed ', to: 'srinivas.tns7@gmail.com,vijju14nov@gmail.com'
}

}
