pipeline {
	agent any
	stages {
	   def mvnHome
	   stage('Preparation') { // for display purposes
		  // Get some code from a GitHub repository
		  git 'https://github.com/jglick/simple-maven-project-with-tests.git'
		  // Get the Maven tool.
		  // ** NOTE: This 'M3' Maven tool must be configured
		  // **       in the global configuration.           
		  mvnHome = tool 'maven'
	   }
	   stage('Build') {

		  env.JAVA_HOME="${tool 'jdk8'}"
		  env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
		   
		  // Run the maven build
		  if (isUnix()) {
			 sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
		  } else {
			 bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
		  }
	   }
	   stage('Results') {
		  junit '**/target/surefire-reports/TEST-*.xml'
		  archive 'target/*.jar'
	   }
   }
}
