pipeline {
    agent any

    tools {
        maven 'Maven_3_8_4'
    }

    environment {
        IMAGE_NAME = 'asg'
        ECR_REGISTRY = '422523651126.dkr.ecr.us-east-1.amazonaws.com'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Compile and Run SonarCloud Analysis') {
            steps {
                sh '''
                mvn clean verify sonar:sonar \
                  -Dsonar.projectKey=therealilyas-buggywebapp \
                  -Dsonar.organization=therealilyas-buggywebapp \
                  -Dsonar.host.url=https://sonarcloud.io \
                  -Dsonar.token=de6c1bd7a374426bbc6248ca6181855a5af829fa
                '''
            }
        }

        stage('SCA – Snyk') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh 'SNYK_TOKEN=$SNYK_TOKEN snyk test || true'
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
