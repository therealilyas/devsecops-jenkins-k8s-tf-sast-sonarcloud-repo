pipeline {
    agent any

    tools {
        maven 'Maven_3_8_4'
    }

    stages {

        stage('Compile and Run Sonar Analysis') {
            steps {
                sh '''
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=therealilyas \
                    -Dsonar.organization=therealilyas \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.token=4ab967958cb7a6f4338ae85627e912e68eb7f3af
                '''
            }
        }
        
        stage('Run SCA Analysis Using Snyk') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh '''
                        curl -sL https://static.snyk.io/cli/latest/snyk-linux -o snyk
                        chmod +x snyk
                        ./snyk auth $SNYK_TOKEN
                        ./snyk test --maven-project
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                withDockerRegistry(
                    credentialsId: 'dockerlogin',
                    url: ''
                ) {
                    script {
                        app = docker.build("devsecops")
                    }
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry(
                        'https://422523651126.dkr.ecr.us-east-1.amazonaws.com',
                        'ecr:us-east-1:aws-credentials'
                    ) {
                        app.push('latest')
                    }
                }
            }
        }
    }
}
