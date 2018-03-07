#!groovy

node('maven') {
	
	stage ('Checkout') {
		checkout scm
	}

	stage ('JarBuild') {
		sh "mvn package -DskipTests=true"
	}

	stage ('SonarTests') {
		sh "mvn -s configuration/cicd-settings.xml site -DskipTests=true"
		step([$class: 'CheckStylePublisher', unstableTotalAll:'300'])
		step([$class: 'PmdPublisher', unstableTotalAll:'20'])
		step([$class: 'FindBugsPublisher', pattern: '**/findbugsXml.xml', unstableTotalAll:'20'])
		step([$class: 'JacocoPublisher'])
		publishHTML (target: [keepAll: true, reportDir: 'target/site', reportFiles: 'project-info.html', reportName: "Site Report"])
	}

	stage ('PushToNexus') {
		sh "mvn -s configuration/cicd-settings.xml deploy -DskipTests=true"
	}

}

