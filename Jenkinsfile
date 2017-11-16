pipeline {
	agent any
	stages {
	   stage('Preparation') { // for display purposes
          steps { 
			  script {
				  // Get some code from a GitHub repository
				  git 'https://github.com/jglick/simple-maven-project-with-tests.git'
				  // Get the Maven tool.
				  // ** NOTE: This 'M3' Maven tool must be configured
				  // **       in the global configuration.           
				  env.mvnHome = tool 'maven'
			   }
		  }
	   }
	   stage('Build') {
          steps { 
			  script {
				  env.JAVA_HOME="${tool 'jdk8'}"
				  env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
				  
				  exit 1
				   
				  // Run the maven build
				  if (isUnix()) {
					 sh "'${env.mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
				  } else {
					 bat(/"${env.mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
				  }
		      }
		  }
	   }
	   stage('Results') {
          steps { 
		  	  input "Continue?"
			  script {
				  junit '**/target/surefire-reports/TEST-*.xml'
				  archive 'target/*.jar'
		     }
		  }
	   }
   }
   post {
	always {
		echo "All done"
		mail to: 'eduardofer@meta4.com' subject: "Test jenkins" body: "Done with build ${env.BUILD_URL)"
	}
   }
}
