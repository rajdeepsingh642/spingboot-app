
pipeline{
    
    agent any 
    tools {
            maven 'mymaven'
        }
    
    stages {
        
        
        stage('Git Checkout'){
            
            steps{
                
               git branch: 'main', url: 'https://github.com/rajdeepsingh642/spingboot-app.git'
                    
                    
                }
            }
        
        

         stage('build image'){
            steps{
                sh 'docker build -t 192.168.1.226:8083/springboot:$BUILD_ID .'

            }
       }

        stage('push to nexus'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus-token', variable: 'nexus_token')]) {
                   sh "docker login -u admin -p $nexus_token 192.168.1.226:8083"
                   sh "docker push 192.168.1.226:8083/springboot:$BUILD_ID"
                   sh "docker rmi 192.168.1.226:8083/springboot:$BUILD_ID"
                    }
                }
            }
        }
        
        stage('pushin helm chart to nexus'){
           steps{
               script{
                   withCredentials([string(credentialsId: 'nexus-token', variable: 'nexus_token')]) {
                    
                      echo "Packing helm chart"
                      sh "helm package -d ${WORKSPACE}/kubernetes ${WORKSPACE}/kubernetes/myapp"
          
                        sh "curl -u admin:${nexus_token} http://192.168.1.226:8081/repository/helm-repo/ --upload-file ${WORKSPACE}/kubernetes/myapp.tgz -v"
    } 
               }
           }
        }
    }


}  
