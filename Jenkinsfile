pipeline {
    agent any
 
   tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/Jenkinsaks/DevOps-Demo-WebApp.git'
             
          }
        }
  stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
   
stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t avnommunication01:latest .' 
                sh 'docker tag avnommunication01 arunsaxena01/avnommunication01:latest'
                //sh 'docker tag samplewebapp arunsaxena01/avnommunication01:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerhub1", url: "" ]) {
          sh  'docker push arunsaxena01/avnommunication01:latest'
        //  sh  'docker push arunsaxena01/avnommunication01:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
   {
                sh "docker run -d -p 8004:8080 arunsaxena01/avnommunication01"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                echo "hello"
                sh "docker -H ssh://root@52.255.157.89 run -d -p 8006:8080 arunsaxena01/avnommunication01"
 
            }
        }
    }
 }
