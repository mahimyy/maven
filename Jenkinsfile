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
		             withCredentials([string(credentialsId: 'dockerhub_pass', variable: 'dockerhub_pass_var')])
			     sh 'docker login -u technetgalaxy -p $(dockerhub_pass_var)'
			     sh 'docker push technetgalaxy/pipeline-java:$BUILD_TAG'
		     }
		      
		}
      }
}
