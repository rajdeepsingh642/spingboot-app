
pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
               git branch: 'main', url: 'https://github.com/rajdeepsingh642/cicd-java-nexus-helm-app.git'
                    
                    
                }
        }
        
         
        
       
        stage('Identifying using datree in using helm charts'){
            steps{
                script{
                    dir(' kubernetes/myapp ') {
                       withEnv(['DATREE_TOKEN=ed7d4d03-e7f6-4b40-877e-aa8f3fc7ec3c']) {
                         sh 'helm datree test .'
 }
                }
            }
        }
    }
}  

}