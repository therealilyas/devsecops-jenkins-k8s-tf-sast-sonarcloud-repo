pipeline {
  agent any
  tools { 
        maven 'Maven_3_8_4'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=buggdywebapp -Dsonar.organization=buggdywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=7933e393cdc6648d27ca7bdb1b7fe9a2b6452f00'
			}
        } 
       stage('RunSCAAnalysisUsingSnyk') {
		  steps { 
		  			withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
						sh 'mvn snyk:test -fn'
					}
		  }
	

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://145988340565.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
  }  
   
   } 

}
