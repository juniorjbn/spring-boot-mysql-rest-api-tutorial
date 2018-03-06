#!groovy

node('maven') {
	
	stage ('checkout') {
		checkout scm
	}

	stage ('JARbuild') {
		sh "mvn package -DskipTests=true"
	}

}

