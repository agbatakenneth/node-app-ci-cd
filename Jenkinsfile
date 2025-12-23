pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        REPOSITORY_URL = "663395718372.dkr.ecr.us-east-1.amazonaws.com/node-repo"
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
                withCredentials([string(credentialsId: 'SONAR_TOKEN1', variable: 'SONAR_TOKEN1')]) {
                    sh '''
                        docker run --rm \
                        -e SONAR_TOKEN1=${SONAR_TOKEN1} \
                        -v ${pwd}:/usr/src \
                        sonarsource/sonar-scanner-cli \
                        -Dsonar.projectKey=agbaken-org_node_project \
                        -Dsonar.organization=agbaken-org \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://sonarcloud.io 
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Sonar analysis successful'
        }
        failure {
            echo '❌ Build failed. Check logs above for more details'
        }
    }
}
