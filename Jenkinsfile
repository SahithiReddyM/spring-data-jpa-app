pipeline {
    agent any
    stages {
    
    	
		stage('Maven Compile'){
			steps{
	
				echo 'Project compile stage'
	
				sh 'mvn compile'
	
		       	}
	
		}

		stage('Unit Test') {
	
		   steps {
	
				echo 'Project Testing stage'
	
				sh 'mvn test'
	
		       
	
	       		}
	
	   	}

	  
		stage('Jacoco Coverage Report') {
	
	        steps{
	        	
	        	sh 'mvn verify'
	            jacoco()
	
			}
	
		}
	    
	    stage('SonarQube'){

	         steps{
			 withSonarQubeEnv('SonarQube2') {
	
	            sh '''mvn sonar:sonar \
					 -Dsonar.host.url=http://35.175.103.228:9000 \
	 				-Dsonar.login=5623afa01d36ee21531aade59a92bcf60e4c212d'''
	
          			}	
	   		 }

		}
		stage('Quality Gate'){

        timeout(time: 10, unit: ‘MINUTES’) {
              def qg= waitForQualityGate()
            if (qg.status!= ‘OK’){
                error “Pipeline aborted due to quality gate failure: ${qg.status}”
            }
        }         
              echo ‘Quality Gate Passed’

    }


        
        stage('Maven Package'){

			steps{
	
				echo 'Project packaging stage'
	
				sh 'mvn package'
	
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


stage('SonarQube'){

         steps{
		 withSonarQubeEnv('SonarQube') {

            bat label: '', script: '''mvn sonar:sonar'''

          }
	 }

	}
