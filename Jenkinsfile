pipeline {
    agent any

    tools {
        maven 'Maven_3_8_4'
    }

    stages {

    	
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=therealilyas-key -Dsonar.organization=therealilyas -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=90fc937f47a642c4d7ecd7ce65cf944b34995fde'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
	            steps {		
					withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
						sh 'mvn snyk:test -fn'
					}
				}
	    }		

        stage('Docker Build') {
            steps {
                script {
                    app = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    docker.withRegistry("https://${ECR_REGISTRY}", 'aws-ecr-creds') {
                        app.push('latest')
                    }
                }
            }
        }
		
    }
}
