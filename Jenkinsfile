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
		
		stage("BuildImage") {
			steps {
				echo "Creating docker image and pusing to docker hub ..."
				sh 'docker build -f Dockerfile -t parnavi/survey-form-jenkins:latest'
			}
		}
		
		stage("PublishImage"){
			steps {
				withDockerRegistry([credentialsId: "$DOCKER_CREDS", url:""]) {
						sh 'docker push parnavi/survey-form-jenkins:latest'
				}
			}
		}
	}
}
