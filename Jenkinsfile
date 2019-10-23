pipeline {
	agent { docker {image  'openjdk' }	}
    stages {
        stage('build') {
			//Consider modifying this to use Kubernetes pod instead of doceker image
            steps {
				echo 'Creating the Jar ...'
				sh 'java -version'
				sh 'jar -cvf surveyform.war * '
				sh 'ls -lrt'
            }
        }
	}
}
