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

	stage ('BuildDEV') {
	// delete old things
		sh "oc -n dev delete buildconfig,deploymentconfig,service -l app=notes"
    // create build. override the exit code since it complains about exising imagestream
	//	sh "oc -n dev new-build --name=notes --image-stream=redhat-openjdk18-openshift:1.0 --binary=true --labels=app=notes || true"
    // build image
        sh "oc -n dev start-build notes --from-file=target/ROOT-1.0.war --wait=true"
    // deploy image
    //	sh "oc -n dev rollout latest notes"
	}

}
