pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="814793384928"
        AWS_DEFAULT_REGION="us-east-1" 
        IMAGE_REPO_NAME="mycertimage"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "814793384928.dkr.ecr.us-east-1.amazonaws.com"
    }
    stages {
        stage('Build') {
            steps {
				sh 'git clone "https://github.com/nandakgnk/Newproj.git"'
				sh 'docker build . -t $IMAGE_REPO_NAME:$IMAGE_TAG -f Newproj/Dockerfile'
            }
        }
        stage('Push to ECR') {
            steps {
                    withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    credentialsId: 'jenkins-aws-int-cred',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $REPOSITORY_URI'
                    sh 'docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $REPOSITORY_URI/$IMAGE_REPO_NAME:$IMAGE_TAG'
                    sh 'docker push $REPOSITORY_URI/$IMAGE_REPO_NAME:$IMAGE_TAG'
                }
            }
        }
    }
}
