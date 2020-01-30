pipeline {

	agent any 
	
	tools {
		maven 'MAVEN'
	}
	
	stages {
		
		stage("Build") {
			steps {
				bat label: '', script: 'mvn clean package'
			}
			post {
				success {
					echo "An artifact is generated successfully."
					echo "Archiving an artifact"
					archiveArtifacts '**/*.war'
				}
				
				failure {
					echo "Failed to generate an artifact"
				}
			}			
		}
		
		stage("Deploy to staging and checking code quality") {
			
			parallel {
			
				stage("Deploy to staging") {
					steps {
						deploy adapters: [tomcat9(credentialsId: '0f46bc4d-25cf-4c1b-8752-4a2bb0f32591', path: '', url: 'http://localhost:8085')], contextPath: null, war: '**/*.war'
					}
					post {
						success {
							echo "An application is deployed to staging environment successfully."
						}
						failure {
							echo "Failed to deploy an application to staging environment"
						}
					}
				}
				
				stage("Checking code quality") {
					steps {
						bat label: '', script: 'mvn checkstyle:checkstyle'
						checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
					}					
				}
			}
		}
		
		stage("Deploy to production") {
			
			steps {
				timeout(1) {
					input 'Do you want to deploy an application to production?'
				}
				
				deploy adapters: [tomcat9(credentialsId: '0f46bc4d-25cf-4c1b-8752-4a2bb0f32591', path: '', url: 'http://localhost:8086')], contextPath: null, war: '**/*.war'
			}
			post {
				success {
					echo "An application is deployed to production environment successfully"
				}
				failure {
					echo "Failed to deploy an application in production"
				}
			}		
		}
	}

}






