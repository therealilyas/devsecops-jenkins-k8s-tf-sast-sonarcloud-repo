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

	// stage('RunSCAAnalysisUsingSnyk') {
	//             steps {		
	// 				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
	// 					sh 'mvn snyk:test -fn'
	// 				}
	// 			}
	//     }		

 //        stage('Build') { 
 //            steps { 
 //               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
 //                 script{
 //                 app =  docker.build("asg")
 //                 }
 //               }
 //            }
 //    }

	// stage('Push') {
 //            steps {
 //                script{
 //                    docker.withRegistry('https://422523651126.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
 //                    app.push("latest")
 //                    }
 //                }
 //            }
 //    	}
		
    }
}
