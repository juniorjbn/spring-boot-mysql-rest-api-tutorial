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
		sh "oc delete buildconfig,deploymentconfig -l app=notes -n dev"
    // create build. override the exit code since it complains about exising imagestream
		sh "oc new-build --name=notes --image-stream=redhat-openjdk18-openshift:1.0 --binary=true --labels=app=notes -n dev || true"
    // build image
        sh "oc start-build notes --from-file=target/ROOT-1.0.jar --wait=true -n dev"
    // deploy image
    //	sh "oc -n dev rollout latest notes"
	}

}
