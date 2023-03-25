
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
                   withCredentials([string(credentialsId: 'nexus-token', variable: 'nexus_creds')]) {
                     dir(' kubernetes/myapp ') {
                       sh "curl -u admin:${nexus_creds}  http://192.168.1.223:8081/repository/helm-repo/ --upload-file myapp-${helmversion}.tgz -v"
                     }  

                   }
               }
           }
        }
    }   
}  


         //stage('pushin helm chart to nexus'){
           // steps{
             //   script{
               //     withCredentials([string(credentialsId: 'nexus-token', variable: 'nexus_creds')]) {
                 //    dir(' kubernetes/myapp ') {
                   //    sh "curl -u admin:${nexus_creds}  http://192.168.1.70:8081/repository/helm-repo/ --upload-file myapp-${helmversion}.tgz -v"
//}
  //              }
    //        }
      //   }


    
        
        //echo "Packing helm chart"
          //  sh "helm package -d ${WORKSPACE}/helm ${WORKSPACE}/helm/devopsodia"
           // sh "${WORKSPACE}/build.sh --pack_helm --push_helm --helm_repo ${HELM_REPO} --helm_usr ${HELM_USR} --helm_psw ${HELM_PSW}"
           //sh "curl -u admin:admin http://192.168.56.120:8081/repository/argocd-helm-repo/ --upload-file ${WORKSPACE}/helm/devopsodia-1.tgz -v"
       