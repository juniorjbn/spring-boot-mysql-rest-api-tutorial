#!groovy

node('maven') {
	
	stage ('checkout') {
		checkout scm
	}

	stage ('JARbuild') {
		sh "ls -lah"
		sh "mvn package"
	}

}
