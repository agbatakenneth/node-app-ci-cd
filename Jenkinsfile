pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        REPOSITORY_URL = "663395718372.dkr.ecr.us-east-1.amazonaws.com/node-repo"
        IMAGE_TAG = "$BUILD_NUMBER"
    }

    tools {
        nodejs 'node18'
        jdk 'jdk17'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SonarCloud scan') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        docker run --rm \
                        -e SONAR_TOKEN=$SONAR_TOKEN \
                        -v $(pwd):/usr/src \
                        sonarsource/sonar-scanner-cli \
                        -Dsonar.projectKey=kens-org_node-app \
                        -Dsonar.organization=kens-org \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://sonarcloud.io 
                    '''
                }
            }
        }
        stage('Login to ECR') {
            steps {
                withCredentials([aws(credentialsId: 'AWS-ECR-CRED', accessKeyVariable:
                'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']) {
                    sh '''
                        aws ecr get-login-password --region $AWS_DEFAULT_REGION \
                         | docker login --username AWS --password-stdin $REPOSITORY_URL
                    '''
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t my-app:$IMAGE_TAG app/
                    docker my-app:$IMAGE_TAGE $REPOSITORY_URL:$IMAGE_TAG
                '''
            }
        }
        stage('Push to ECR') {
            steps {
                sh '''
                    docker push $REPOSITORY_URL:$IMAGE_TAG
                    kubectl set image deployment/my-app my-app=$REPOSITORY_URL:$IMAGE_TAG
                '''
            
            }
        }
    }    

    post {
        success {
            echo '✅ Sonar analysis successfully completed'
            echo '✅ Build and Docker Push successfully'
            echo ' Pushed Image: $REPOSITORY_URL:$IMAGE_TAG'
        }
        failure {
            echo '❌ Build failed. Check logs above for more details and further troubleshooting'
        }
    }
}
