node {    
      def app1 
      def app2   
      stage('Clone repository') {               
             
            checkout scm    
      }
      parallel {
                  stage('Build cast-service image') {         
                        app1 = docker.build("mateuszptrk/cast-service", "./cast-service")   
                   }     
                  
                     stage('Build movie-service image') {         
                        app2 = docker.build("mateuszptrk/movie-service", "./movie-service")    
                   }     
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
        }
