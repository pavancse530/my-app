node{
   stage('GIT Checkout'){
     git 'https://github.com/damodaranj/my-app.git'
   }
   stage('Compile-Package using maven'){
    
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('Build Docker Imager'){
   sh 'docker build -t damocharms/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u damocharms -p ${dockerPassword}"
   }
   sh 'docker push damocharms/myweb:0.0.2'
   }
    stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest damocharms/myweb:0.0.2' 
   }
}
   }
