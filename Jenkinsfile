#!groovy

node('maven') {
	checkout()
	JARbuild()
	Nexus()
	IMGbuild()
	DEVdeploy()
	STGdeploy()
}

/* ### def stages ### */

def checkout () {
	stage 'Checkout'
	deleteDir()
	checkout scm
}

def JARbuild () {
	sh "mvn package"
}

def Nexus () {
	sh "echo uploading"
}