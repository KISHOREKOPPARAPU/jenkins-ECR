pipeline {
    agent any
     environment {
        AWS_ACCOUNT_ID = '566829558047'
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_REPOSITORY_NAME = 'jenkins-ecr-repo'
        IMAGE_TAG = "latest"
     }
   
    stages {
          stage('Checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/KISHOREKOPPARAPU/jenkins-ECR.git'
            }
        }
           stage('Building image') {
             steps{
                  script {
                   dockerImage = docker.build registry
                   }
      }
           }
    
            stage('Pushing to ECR') {
             steps{  
                  script {
     withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_cred', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
     sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 566829558047.dkr.ecr.us-east-1.amazonaws.com'
     sh 'docker push 566829558047.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr-repo:latest'
}

}
                  }
            }
             stage('stop previous containers') {
               steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
            stage('Docker Run') {
              steps{
                   script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 566829558047.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr-repo:latest'     
      }
    }
        }
    }
  }
