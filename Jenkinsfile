node{
	
	def mavenHome = tool name: "maven-3.8.2"
  
       echo "GitHub BranhName ${env.BRANCH_NAME}"
       echo "Jenkins Job Number ${env.BUILD_NUMBER}"
       echo "Jenkins Node Name ${env.NODE_NAME}"
  
       echo "Jenkins Home ${env.JENKINS_HOME}"
       echo "Jenkins URL ${env.JENKINS_URL}"
       echo "JOB Name ${env.JOB_NAME}"
	
	stage('CheckoutCode')
	{
		git branch: 'development', credentialsId: '80f08865-bf8e-4f1b-86e8-02f4e3fa01c7', url: 'https://github.com/sonu00000/maven-web-application.git'
	}
	stage('Build')
	{
		sh "${mavenHome}/bin/mvn clean package"
	}
	stage('SonarQubeReport')
	{
		sh "${mavenHome}/bin/mvn clean sonar:sonar"
	}
	stage('UploadArtifactIntoNexus')
	{
		sh "${mavenHome}/bin/mvn clean deploy"
	}
	stage('DeployAppIntoTomcatServer')
	{
		sshagent(['9f955572-4fe2-4d00-a2d8-045e11238f52']) {
			sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.211.79:/opt/apache-tomcat-9.0.53/webapps/"
		}
	}
	
/*	
    stage('SendEmailNotification') {
		emailext body: '''Build is over.!

		Regards,
		TCS,
		6664525123''', subject: 'Build Over...!!', to: 'banik.soumajit@gmail.com'
	}*/
	
}
