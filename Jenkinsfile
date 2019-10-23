pipeline {
    stages {
        stage('build') {
			//Consider modifying this to use Kubernetes pod instead of doceker image
			agent { docker 'openjdk:8-jre' }	
            steps {
				echo 'Creating the Jar ...'
				sh 'java -version'
				sh 'jar -cvf surveyform.war * '
				sh 'ls -lrt'
	    }
        }
   }
}
