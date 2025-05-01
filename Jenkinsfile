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
      }
   }
