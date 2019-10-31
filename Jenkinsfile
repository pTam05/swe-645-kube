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
				sh 'jar -cvf surveyform.war *'
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

		stage("UpdateDeployment") {
			steps{
				script{
					echo "Updating Deployment"
					sh "kubectl config view"
					
					withKubeConfig([credentialsId:'jenkins-deployer-creds', clusterName:'gke_swe-645-kube_us-east4-a_kube-cluster', serverUrl: 'https://35.199.47.233']) {
						sh "kubectl config view"
						sh "kubectl get deployments"
					}
				}
			}
		}
	}
}
