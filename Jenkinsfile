
pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
               git branch: 'main', url: 'https://github.com/rajdeepsingh642/cicd-java-nexus-helm-app.git'
                    
                    
                }
        }
        
         
        
        stage('Sonar Quality check'){
         agent {
                docker { 
                   image  'maven'
            }
            }
            steps{
                script{
                
                withSonarQubeEnv(credentialsId: 'sonar-qube') {
                   sh "mvn clean install sonar:sonar"
                    }
              }
            }
              
                       
        }
                    
         
        stage('Docker build'){
            steps{
                script{
                    sh 'docker build -t rajdeepsingh642/springboad:$BUILD_ID .'
                  
                }
            }
        }
        
        stage('Docker push') {
             steps{
                script{
                     withCredentials([string(credentialsId: 'dockerhub', variable: 'docker_id')]) {


                      sh "docker login -u rajdeepsingh642 -p ${docker_id}"
                     }
                      sh  'docker push rajdeepsingh642/springboad:$BUILD_ID'
                       

                      
                       }
                
             }
          }
                
        stage('Identifying using datree in using helm charts'){
            steps{
                scrips{
                    dir(' kubernetes/myapp ') {
                       withEnv(['DATREE_TOKEN=5644d4f3-5357-4ad4-97c5-f42e0459c250']) {
                         sh 'helm datree test .'
 }
                }
            }
        }
    }

}  

}