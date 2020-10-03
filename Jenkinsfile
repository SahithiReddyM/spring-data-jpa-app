pipeline {
  agent any
  stages {
  
	stage('Maven Compile'){
		steps{
			echo 'Project compile stage'
			bat label: 'Compilation running', script: '''mvn compile'''
	       	}
	}
	
	stage('Unit Test') {
	   steps {
			echo 'Project Testing stage'
			bat label: 'Test running', script: '''mvn test'''
	       
            }
   	}
	
        
    stage('Jacoco Coverage Report') {
        steps{
            jacoco()
		}
	}
        
	stage('SonarQube'){
         steps{
            bat label: '', script: '''mvn sonar:sonar \
		 -Dsonar.host.url=http://3.238.72.11:9000 \
 		-Dsonar.login=5623afa01d36ee21531aade59a92bcf60e4c212d'''
          }
	}
	
	stage('Maven Package'){
		steps{
			echo 'Project packaging stage'
			bat label: 'Project packaging', script: '''mvn package'''
		}
	}
  	
    
  }
  environment {
        EMAIL_TO = 'mallusahithireddy5@gmail.com'
    }
  post {
        success {
            emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        }
	    failure {
            emailext attachLog:true,body: 'Check console output at $BUILD_URL to view the results.', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
    
    }
}
