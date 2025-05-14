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
                                      
		              sh 'sudo docker run -dt --name web4tom -p 8087:8080 technetgalaxy/pipeline-java:$BUILD_TAG'
                    } 
	       }
	       stage("test-website") {
	             steps { 
		              sh 'sudo curl http://18.191.240.40:8087'
                       }
	       }
	        stage("approval stage") {
                     steps {
                         script {
                                 Boolean userInput = input(id: 'Proceed1', message: 'Do you want Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm your agree with this']])}
                                 echo 'userInput: ' + userInput
                        }
                }
	 }
}
