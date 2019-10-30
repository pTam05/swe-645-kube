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
				sh 'jar -tvf surveyform.war'
            }
        }
		
		stage("BuildPublishImage"){
			steps {
				script {
					echo "${env.BUILD_ID}"
					img = docker.build "parnavi/survey-form-image-gcp:${env.BUILD_ID}"

					withDockerRegistry(credentialsId: 'docker', url: '') {
						echo "Creating docker image and pusing to docker hub ..."

						img.push "${env.BUILD_ID}"
					}
				}
			}
		}
	}
}
