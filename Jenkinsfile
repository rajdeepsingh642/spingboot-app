
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
                    
         stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-qube'       
                   
                        
                    }     
        }          
                       
        }

        stage('Docker build$ push'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus-token', variable: 'nexus-build')]) {
                      sh '''
                       docker build -t 192.168.1.70/springapp:$BUILD_ID .
                       docker login -u admin -p $nexus-build 192.168.1.70:8083
                       docker image push 192.168.1.70/springapp:$BUILD_ID
                       docker image rm 192.168.1.70/springapp:$BUILD_ID
                       '''

                      
}
                }
            }
        }        
    }
}  

