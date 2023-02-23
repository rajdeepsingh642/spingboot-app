
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
                   withCredentials([string(credentialsId: 'rajdeepsingh642', variable: 'docker-hub')]) {
    // some block

                      sh '''
                       docker build -t rajdeepsingh642/springboad:$BUILD_ID .
                       docker login -u rajdeepsingh642 -p ${docker-hub}
                       docker image push rajdeepsingh642/springapp:$BUILD_ID
                       '''

                      
                       }
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