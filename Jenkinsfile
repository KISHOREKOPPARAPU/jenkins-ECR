pipeline {
    agent any
     environment {
        registry = "566829558047.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr-repo"
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
               withCredentialsRegistry(CredentialId:'docker','toolName: 'docker') {
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
