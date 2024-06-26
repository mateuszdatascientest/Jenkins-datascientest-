node {    
      def app1 
      def app2

      stage('Clone repository') {               
             
            checkout scm    
      }

                  stage('Build cast-service image') {         
                        app1 = docker.build("mateuszptrk/cast-service", "./cast-service")   
                   }     
                  
                     stage('Build movie-service image') {         
                        app2 = docker.build("mateuszptrk/movie-service", "./movie-service")    
                   }     

       stage('Push cast-service image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
              app1.push("${env.BUILD_NUMBER}")            
              app1.push("latest")        
              }    
           }

       stage('Push movie-service image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
              app2.push("${env.BUILD_NUMBER}")            
              app2.push("latest")        
              }    
           }

        stage('Convert to K8S') {
		sh ' curl -L https://github.com/kubernetes/kompose/releases/download/v1.33.0/kompose-linux-amd64 -o kompose '
		sh ' chmod +x kompose'
		sh '  ./kompose convert -f  docker-compose.yml'
		sh '  mkdir -p manifests'
		sh '  mv *.yaml manifests'
        }
	
stage('Deploy to dev') {
		       script {
    FULL_PATH_BRANCH = sh(script:'git name-rev --name-only HEAD', returnStdout: true)		       
    if ( FULL_PATH_BRANCH == 'dev') {
	echo env.BRANCH_NAME    
		        sh ' kubectl apply -f manifests/ -n dev '
    } 
		       }
}

stage('Deploy to QA') {
		       script {
    FULL_PATH_BRANCH = sh(script:'git name-rev --name-only HEAD', returnStdout: true)		       
    if ( FULL_PATH_BRANCH == 'qa') {
	echo env.BRANCH_NAME    
		        sh ' kubectl apply -f manifests/ -n qa '
    } 
		       }
}

	stage('Deploy to Staging') {
		       script {
    FULL_PATH_BRANCH = sh(script:'git name-rev --name-only HEAD', returnStdout: true)		       
    if ( FULL_PATH_BRANCH == 'staging') {
	echo env.BRANCH_NAME    
		        sh ' kubectl apply -f manifests/ -n staging '
    } 
		       }
}
	
       stage('Deploy to production') {
	       script {
		FULL_PATH_BRANCH = sh(script:'git name-rev --name-only HEAD', returnStdout: true)		       
	        if ( FULL_PATH_BRANCH == 'main') {
			input 'Do you approve deployment to production?'
		        sh ' kubectl apply -f manifests/ -n prod '
		}      
	 }
        }
	
        }
