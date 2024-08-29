pipeline {
	agent any
	environment {
 		 mvn = "/home/ubuntu/apache-maven-3.9.9/bin/mvn"
		}

	stages {
		stage ('compile') {
			steps {
				echo "sai"
				sh 'ls -l'
				echo "checking mvn is installed or not"
				sh '${mvn} -h'
				echo "compiling the code"
				sh '${mvn} package'
				}
			}
		}
	}
