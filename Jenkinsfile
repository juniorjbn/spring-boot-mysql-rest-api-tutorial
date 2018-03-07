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
		sh "cp target/ROOT-1.0.war oc-build/deployments/ROOT.war"
    // create build. override the exit code since it complains about exising imagestream
		sh "oc new-build --name=notes --image-stream=redhat-openjdk18-openshift:1.0 --binary=true --labels=app=notes -n dev || true"
    // build image
        sh "oc start-build notes --from-dir=oc-build --wait=true -n dev"
    // deploy image
    // usar o plugin para oc rollout latest notes


	}

}

