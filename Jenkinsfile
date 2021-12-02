pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="635776503252"
        AWS_DEFAULT_REGION="us-east-1" 
        IMAGE_REPO_NAME="jenkins-pipline-build-demo"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "https://https://console.aws.amazon.com/ecr/repositories?region=us-east-1"
    }
   
    stages {
        
        stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr-public get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin public.ecr.aws/r9s5q7k2"
                }
                 
            }
        }
    
        
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Armen8793/MyTask.git']]])    
            }
        }
  
    // Building Docker images
        stage('Building image') {
            steps{
                script {
                dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }   
            }
        }
   
    // Uploading Docker images into AWS ECR
        stage('Pushing to ECR') {
            steps{  
                script {
                sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }
    } 
}	

