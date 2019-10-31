pipeline {
	agent any
	environment {
		DOCKER_CREDS = credentials('docker')
		DOCKER_IMAGE_NAME = "parnavi/survey-form-image-gcp"
		img = ''
	}
    stages {
	
        stage('BuildWAR') {
            steps {
				echo 'Creating the Jar ...'
				sh 'java -version'
				sh 'jar -cvf surveyform.war *'
				sh 'ls -lrt'
				sh 'jar -tvf surveyform.war'
            }
        }
		
		stage("BuildPublishImage"){
			steps {
				script {
					echo "${env.BUILD_ID}"
					img = docker.build(DOCKER_IMAGE_NAME)

					withDockerRegistry(credentialsId: 'docker', url: '') {
						echo "Creating docker image and pusing to docker hub ..."

						img.push "${env.BUILD_ID}"
					}
				}
			}
		}

		stage("UpdateDeployment") {
			steps{
				
				withKubeConfig([credentialsId:'kube-creds-jenkins', serverUrl: 'https://35.199.47.233']) {
						sh "kubectl config view"
						sh "kubectl get deployments"
					}
			}
			
			
		}
	}
}
