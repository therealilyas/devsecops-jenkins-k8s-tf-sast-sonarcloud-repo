pipeline {
    agent any

    tools {
        maven 'Maven_3_8_4'
    }

    environment {
        SONAR_HOST_URL = 'https://sonarcloud.io'
        SONAR_PROJECT_KEY = 'buggywebapp'
        SONAR_ORG = 'buggywebapp'
        ECR_REGISTRY = '422523651126.dkr.ecr.us-east-1.amazonaws.com'
        IMAGE_NAME = 'asg'
    }

    stages {

        stage('Build & SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                      -Dsonar.organization=$SONAR_ORG \
                      -Dsonar.host.url=$SONAR_HOST_URL \
                      -Dsonar.token=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('SCA – Snyk') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh '''
                    snyk auth $SNYK_TOKEN
                    snyk test || true
                    '''
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
