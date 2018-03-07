#!groovy

node('maven') {
	
	stage ('Checkout') {
		checkout scm
	}

	stage ('JarBuild') {
		sh "mvn package -DskipTests=true"
	}

	stage ('SonarTests') {
		sh "mvn clean package sonar:sonar"
	}

	stage ('PushToNexus') {
		sh "mvn -s configuration/cicd-settings.xml deploy -DskipTests=true"
	}

}

