pipeline {
	
	agent any
	
	tools {
		maven 'MAVEN'
	}
	
	stages {
	
		stage("Package") {
			steps {
				bat label: '', script: 'mvn clean package'
			}
			post {
				success {
				    echo "Artifact is generated successfully."
					echo "Archiving an artifact......"
					archiveArtifacts '**/*.war'
				}
				failure {
					echo "Something went wrong...."
				}
			}			
		}
		
		stage("Deploy to staging") {
			steps {
				build 'maven-webapp-deploy-to-staging'
			}
			post {
				success {
					echo "An application is deployed to staging environment successfully"
				}
				failure {
					echo "Failed to deploye an application to staging environment"
				}
			}
		}
		
		stage("Checking code quality") {
			steps {
				build 'maven-webapp-checkstyle'
			}
		}

		stage("Deploy to production") {
			steps {
				timeout(time: 5, unit: 'HOURS') {
					input 'Deploy an application to production?'
				}
				
				build 'maven-webapp-deploy-to-production'
			}
			
			post {
				success {
					echo "An application is deployed to production successfully"
				}
				failure {
					echo "Failed to deploy an application to production"
				}
			}
		}		
	}
}









