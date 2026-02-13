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
  }
}
