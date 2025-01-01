pipeline {
	agent any
	environment {
 		 mvn = "/home/ubuntu/apache-maven-3.9.9/bin/mvn"
	  	 acr_cred = credentials('docker hub')
		  registry = 'http://172.31.38.95/artifactory'
		 
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
				sh 'docker tag testimage:latest dockerhubtestsai.azurecr.io/samples/testimage:v1.0.0'
				}
			}
	stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:${registry}" ,  credentialsId:"artifactory_token"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }  
		stage('push image') {
			steps {
			
			sh 'docker login -u $acr_cred_USR -p $acr_cred_PSW dockerhubtestsai.azurecr.io'
 			sh 'docker push dockerhubtestsai.azurecr.io/samples/testimage:v1.0.0' 		 

			}
		}
	}
	}



