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
                'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
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
                    docker tag my-app:$IMAGE_TAG $REPOSITORY_URL:$IMAGE_TAG
                '''
            }
        }
        stage('Push Image to ECR') {
            steps {
                sh '''
                    docker push $REPOSITORY_URL:$IMAGE_TAG              
                '''
            
            }
        }
        stage('Deploy to Kubernetes'){
            steps {
                withCredentials([file(credentialsId: 'KUBECONFIG_DEVOPS', variable:
                'KUBECONFIG'),
                aws(credentialsId: 'AWS-ECR-CRED', accessKeyVariable:
                'AWS_ACCESS_KEY_ID', secrestKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG
                        export AWS_DEFAULT_REGION=us-east-1
                        echo "installing prometheus monitor...."
                        helm repo add prometheus-community https://prometheus-community.github.io/helm-charts || true
                        

                        helm repo update
                        helm upgrade --install prometheus \
                         prometheus-community/kube-prometheus-stack \
                          --namespace monitoring \
                          --create-namespace

                        echo "updating image tag in deployment.yaml..."
                        sed -i "s|ECR_URI:latest|${REPOSITORY_URI}:${IMAGE_TAG}|g" 
                        k8s/deployment.yaml

                        echo "Applying kubernetes manifests...."
                        kubectl apply -f k8s/

                        echo "veryfying rollout...."
                        kubectl rollout status deployment/node-app
                        '''
                }
            }
        }
    }    

    post {
        success {
            echo '✅ Sonar analysis successfully completed'
            echo '✅ Build and Docker Push successfully'
            echo ' Pushed Image: $REPOSITORY_URL:$IMAGE_TAG'
            echo '✅ Kubernetes Deployment successful'
        }
        failure {
            echo '❌ Build failed. Check logs above for more details and further troubleshooting'
        }
    }
}
