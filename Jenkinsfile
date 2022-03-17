pipeline {
    agent any

    stage('SCM Checkout'){
       git credentialsId: 'git-creds', url: 'https://github.com/Shaik2khaleel/project.git'
   }
   stage('Mvn Package'){
     def mvnHome = tool name: 'maven-3', type: 'maven'
     def mvnCMD = "${mvnHome}/bin/mvn"
     sh "${mvnCMD} clean package"
   }
   stage('Build Docker Image'){
     sh 'docker build -t shaikkhaleeljibran/WebAppCal-1.3.5 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
        sh "docker login -u shaikkhaleeljibran -p ${dockerHubPwd}"
     }
     sh 'docker push shaikkhaleeljibran/WebAppCal-1.3.5'
   }
   stage('Run Container on Dev Server'){
     def dockerRun = 'docker run -p 8080:8080 -d --name my-app shaikkhaleeljibran/WebAppCal-1.3.5'
     sshagent(['dev-server']) {
       sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.38.76 ${dockerRun}"
     }
   }
}
