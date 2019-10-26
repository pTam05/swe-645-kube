pipeline {
	agent any
	environment {
		DOCKER_CREDS = credentials('docker')
		img = ''
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
		
		stage("BuildPublishImage"){
			steps {
				script {
					img = docker.build 'parnavi/survey-form-jenkins:latest'

					withDockerRegistry(credentialsId: 'dockerHubId', url: '') {
						echo "Creating docker image and pusing to docker hub ..."

						img.push 'latest'
					}
				}
			}
		}
	}
}
