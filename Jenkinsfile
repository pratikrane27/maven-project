pipeline {
	agent any 
	tools {
		maven "maven"
	}
	stages {
		stage("Build") {
			steps {
				echo "Building an application using maven"
				sh 'mvn clean package'
			}
			post {
				success {
					echo "Artifact is generated succesfully"
					echo "Archiving artifact"
					archiveArtifacts artifacts: '**/*.war', followSymlinks: false
				}
				failure {
					echo "Failed to generate an artifact"
				}
			}
		}
		stage("Deploy to staging and checking code") {
		parallel {
			stage("Deploy to staging") {
				steps {
					deploy adapters: [tomcat8(credentialsId: '19acb682-f71a-4890-b9f9-9b402fe63546', path: '', url: 'http://ec2-18-222-212-66.us-east-2.compute.amazonaws.com:9999/')], contextPath: null, onFailure: false, war: '**/*.war'
				}
				post {
					success {
						echo "App deployed succesfully on tomcat staging"
					}
					failure {
						echo "Failed to deploy app on tomcat staging"
					}
				}
			}
			stage("Performing code analysis") {
				steps {
					sh 'mvn checkstyle:checkstyle'
				}
				post {
					success {
						checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
					}
				}
			}
		}
	}
			stage("Deploy to production") {
				steps {
					timeout(time: 1, unit: 'DAYS') {
						input 'Do you want to deploy application to production'
			}
					deploy adapters: [tomcat8(credentialsId: '19acb682-f71a-4890-b9f9-9b402fe63546', path: '', url: 'http://ec2-18-222-212-66.us-east-2.compute.amazonaws.com:9090/')], contextPath: null, onFailure: false, war: '**/*.war'
		}
	}
	post {
		success {
			echo "App is deployed succesfully in production"
		}
		failure {
			echo "failed to deploy app in production"
		}
	}
		
	}
	
	
}