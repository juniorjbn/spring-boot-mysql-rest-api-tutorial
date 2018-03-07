#!groovy

node('maven') {
	
	stage ('Checkout') {
		checkout scm
	}

	stage ('JarBuild') {
		sh "mvn package -DskipTests=true"
	}

	stage('SonarQube analysis') { 
		def scannerHome = tool 'sonarqube';
		withSonarQubeEnv('sonarqube') {
		sh "${scannerHome}/bin/sonar-scanner"
		sh "sleep 10"
		}
		def qualitygate = waitForQualityGate();
			if (qualitygate.status != "OK") {
			error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
		}  
    }

	stage ('PushToNexus') {
		sh "mvn -s configuration/cicd-settings.xml deploy -DskipTests=true"
	}

}

