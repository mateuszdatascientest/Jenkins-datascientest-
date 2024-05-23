node {    
      def app     
      stage('Clone repository') {               
             
            checkout scm    
      }     
      stage('Build image') {         
       
            app = docker.build("mateuszptrk/cast-service", "./cast-service")    
       }     
   
       stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
              app.push("${env.BUILD_NUMBER}")            
              app.push("latest")        
              }    
           }
        }
