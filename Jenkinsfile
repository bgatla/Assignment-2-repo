pipeline {
    agent any
    tools {
        
        maven 'maven_3_8_6'
    }
    
    stages {
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/bgatla/Assignment-2-repo']]])
                sh 'mvn clean install'
            }
        }
    stage('Docker Build and Tag') {
           steps {
              
              // building the docker image with name mysurvey
                sh 'docker build -t mysurvey .' 
                
                // we’re going to tag our new image and tag name is latest
                sh 'docker image tag mysurvey bgatla/mysurvey:latest'
               
          }
        }
    stage('Push docker image to docker hub') {
        steps {
            withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                // providing credentials to login into dockerhub with configured credentials
            sh 'docker login -u bgatla -p ${dockerhubpwd}'
           }
           // pushing image to dockerhub
        sh 'docker image push bgatla/mysurvey:latest'
        }
    }
    stage('Deploying to k8s')
    {
        steps {
            sh 'kubectl set image deployment/clusterdeploy container-0=bgatla/mysurvey:latest -n default'
            sh 'kubectl rollout restart deploy clusterdeploy -n default'
        }
    }
 }
}