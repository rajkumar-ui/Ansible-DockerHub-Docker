node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/rajkumar-ui/Docker-Web-App.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.5.6", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t nani903020/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'a89d8b3f-b337-4b3f-8421-b21e9a11c3b2', variable: 'dockerhubpwd')]) {
          sh "docker login -u nani903020 -p ${dockerhubpwd}"
        }
        sh 'docker push nani903020/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app nani903020/java-web-app'
         
         sshagent(['a80eab62-3bec-4b7b-a98d-69563fb57549']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.148 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.12.148 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.12.148 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.12.148 ${dockerRun}"
       }
       
    }
     
     
}
