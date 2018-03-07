#!groovy

node('maven') {
	
	stage ('Checkout') {
		checkout scm
	}

	stage ('JarBuild') {
		sh "mvn package -DskipTests=true"
	}

	stage('Code Analysis') {
        sh "mvn sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -DskipTests=true"
    }

	stage ('PushToNexus') {
		sh "mvn -s configuration/cicd-settings.xml deploy -DskipTests=true"
	}

}

