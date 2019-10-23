pipeline {
	agent any
	environment {
		DOCKER_CREDS = credentials('dockerHubId')
	}
    stages {
	
        stage('BuildWAR') {
			//Consider modifying this to use Kubernetes pod instead of doceker image
            steps {
				echo 'Creating the Jar ...'
				sh 'java -version'
				sh 'jar -cvf surveyform.war src/surveyform.css src/surveyform.html'
				sh 'ls -lrt'
            }
        }
		def img
		stage("BuildPublishImage"){
			steps {
				
				img = docker.build 'parnavi/survey-form-jenkins:latest'
				// This step should not normally be used in your script. Consult the inline help for details.
				//withDockerRegistry(credentialsId: 'dockerHubId', url: '') {
				//	echo "Creating docker image and pusing to docker hub ..."
					
				//	img.push 'latest'
				//}
			}
		}
	}
}
