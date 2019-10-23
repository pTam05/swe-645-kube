pipeline {
	agent any
	environment {
		DOCKER_CREDS = credentials('dockerHubId')
	}
    stages {
	
		 stage('Initialize'){
			steps {
				def dockerHome = tool 'docker'
				env.PATH = "${dockerHome}/bin:${env.PATH}"
			}
		}
		
        stage('BuildWAR') {
			//Consider modifying this to use Kubernetes pod instead of doceker image
            steps {
				echo 'Creating the Jar ...'
				sh 'java -version'
				sh 'jar -cvf surveyform.war src/surveyform.css src/surveyform.html'
				sh 'ls -lrt'
            }
        }
		
		stage("BuildPublishImage"){
			steps {
				// This step should not normally be used in your script. Consult the inline help for details.
				withDockerRegistry(credentialsId: 'dockerHubId', toolName: 'docker') {
					echo "Creating docker image and pusing to docker hub ..."
					sh 'docker build -f Dockerfile -t parnavi/survey-form-jenkins:latest'
					sh 'docker push parnavi/survey-form-jenkins:latest'
				}
			}
		}
	}
}
