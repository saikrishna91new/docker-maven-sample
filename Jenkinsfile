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
		stage ('build image') {
			steps {
				echo "building the docker image"
				sh 'docker build -t testimage:latest .'
				sh 'docker tag testimage:latest dockerhubtestsai.azurecr.io:v1.0.0'
				}
			}
		stage('push image') {
			steps {
			withCredentials([usernamePassword(credentialsId: 'docker hub', passwordVariable: 'acrpswrd', usernameVariable: 'jenkins')]) {
			sh 'docker login dockerhubtestsai.azurecr.io'
 			sh 'docker push dockerhubtestsai.azurecr.io:v1.0.0' 		 
}
			}
		}
	}
	}



