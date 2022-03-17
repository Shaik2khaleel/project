pipeline {
    agent any
  
    tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/Shaik2khaleel/project.git'
             
          }
        }
   stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t Dockerpipeline:latest .' 
                sh 'docker tag Dockerpipeline shaikkhaleeljibran/Dockerpipeline:latest'
                //sh 'docker tag Dockerpipeline shaikkhaleeljibran/Dockerpipeline:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
          sh  'docker push shaikkhaleeljibran/Dockerpipeline:latest'
        //  sh  'docker push shaikkhaleeljibran/Dockerpipeline:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
      {
                sh "docker run -d -p 8003:8080 shaikkhaleeljibran/Dockerpipeline"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.38.76 run -d -p 8080:8080 shaikkhaleeljibran/Dockerpipeline"
 
            }
        }
    }
  }
