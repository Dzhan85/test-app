pipeline {
agent {
    label 'jenkins-slave'
}

environment {
DEPLOY = "${env.BRANCH_NAME == "main" || env.BRANCH_NAME == "develop" ? "true" : "false"}"
NAME = "${env.BRANCH_NAME == "main" ? "example" : "example-staging"}"
AWS_ACCOUNT_ID="your_account"
AWS_DEFAULT_REGION="us-east-1"
IMAGE_REPO_NAME="apps/backend/dev"
IMAGE_TAG="1.0.$BUILD_NUMBER"
GITHUB_TOKEN="$GITHUB_TOKEN"
DOMAIN='localhost'
REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
}

stages {

stage('Cloning Git') {
steps {
    // script {
    //                 properties([pipelineTriggers([pollSCM('* * * * *')])])
    //        }
    checkout([$class: 'GitSCM', branches: [[name: '']], extensions: [], userRemoteConfigs: [[credentialsId: 'eksdev/jenkins/jenkins-ssh-key', url: 'git@github.com:Dzhan85/test-app.git']]])
}
}




stage('Logging into AWS ECR') {
steps {
script {
    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
}

}
}


stage('Docker Build') {
     when {
                environment name: 'DEPLOY', value: 'true'
            }
      steps {
        sh "docker build --build-arg GITHUB_TOKEN=$GITHUB_TOKEN -t your_account.dkr.ecr.us-east-1.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG} -f Dockerfile ." 
      }
}



// Push image into AWS ECR
stage('Pushing to ECR') {
 when {
                environment name: 'DEPLOY', value: 'true'
            }
steps{
script {
sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
}
}
}

stage('Kubernetes Deploy') {
            when {
                environment name: 'DEPLOY', value: 'true'
            }
            steps {
                container('helm') {
                    sh "helm upgrade --install --force --set name=${NAME} --set image.tag=${IMAGE_TAG} --set domain=${DOMAIN} ${NAME} ./helm"
                }
            }
        }

} // End of stages
post {
   success {
      slackSend channel: '#jenkins-builds',
          message: " docker build ${IMAGE_TAG} is successful"
   }
   failure {
      slackSend channel: '#jenkins-builds',
          message: "T docker build ${IMAGE_TAG} has failed"
   }
}
} // End of pipeline
