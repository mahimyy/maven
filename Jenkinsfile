pipeline {
        agent {
               label 'slave1'
               }
          stages {
	        stage("SCM") {
                     steps {
                            git 'https://github.com/technet001/maven-web-application.git'
                            }
                          }

                stage("build") {
                     steps {
                             sh 'sudo mvn clean package'
                             }
                           }
                stage("build-image") {
                     steps {
                             sh 'sudo docker build -t java-repo:$BUILD_TAG .'
                             sh 'sudo docker tag java-repo:$BUILD_TAG technetgalaxy/pipeline-java:$BUILD_TAG'
                             }
                }
                stage("dockerlogin") {
                     steps { 
		             withCredentials([string(credentialsId: 'dockerhub_pass', variable: 'dockerhub_pass_var')]) {
			     sh 'sudo docker login -u technetgalaxy -p ${dockerhub_pass_var}'
			     sh 'sudo docker push technetgalaxy/pipeline-java:$BUILD_TAG'
			     }
		     }
		      
		}
		stage("QAT TESTING") {
		     steps {  
		              sh 'sudo docker rm -f $(sudo docker ps -a -q)'
		              sh 'sudo docker run -dit --name web10tom -p 8090:8080 technetgalaxy/pipeline-java:$BUILD_TAG'
                    } 
	       }


	        stage("test-website") {
	             steps { 
		             sh 'sudo sleep 20'
		             sh 'sudo curl --ipv4 http://localhost:8090'

                       }
	       }
	        stage("Approval-stage") {
                     steps {
                         script {
                                 Boolean userInput = input(id: 'Proceed1', message: 'Do you want Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm your agree with this']])
                                 echo 'userInput: ' + userInput 
				 }
                        }
                }
		stage('deployment to production') {
		      agent {
		            label "node1"
			    }
		      steps {
		          
			  sshagent(['ec2-access-cred']) {
			  
			   sh 'ssh -o StrictHostKeyChecking=no ec2-user@35.171.26.240 sudo docker run -d -p 49153:8080 --name webtest1 technetgalaxy/pipeline-java:$BUILD_TAG'
	         }
             }
	  }
     }
}



















