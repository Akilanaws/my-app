node{
   stage('SCM Checkout'){
     //git 'https://github.com/Akilanaws/my-app.git'
   }
   stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }

     stage('Build Docker Image'){
   sh 'docker build -t 260690/myweb:0.0.5 .'
   }
     
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh 'docker login -u 260690 -p ${dockerPassword}'
    }
   sh 'docker push 260690/myweb:0.0.5'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 9600:8080 --name tomcattest 260690/myweb:0.0.5' 
   }
}
}
