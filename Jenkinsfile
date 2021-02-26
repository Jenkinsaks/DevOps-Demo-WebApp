pipeline {
  agent any

// using the Timestamper plugin we can add timestamps to the console log
  options {
    timestamps()
  }
    tools { 
       // maven 'Maven 3.3.9' 
        //jdk 'jdk8' 
    }
	
  environment {
    //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
    IMAGE = readMavenPom().getArtifactId()
    VERSION = readMavenPom().getVersion()
  }
	
  stages {
    stage("Checkout Project") {
      steps {

        // Script blocks can run any Groovy script
	//checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, 
	//	  extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/arunsaxena01/DevOps-Demo-WebApp.git']]])
      }

    }
	
stage('Build') {
      agent {
        any {
          /*
           * Reuse the workspace on the agent defined at top-level of Pipeline but run inside a container.
           * In this case we are running a container with maven so we don't have to install specific versions
           * of maven directly on the agent
           */
          //reuseNode true
          //image 'maven:3.5.0-jdk-8'
        }
      }
      steps {
        // using the Pipeline Maven plugin we can set maven configuration settings, publish test results, and annotate the Jenkins console
        withMaven(options: [findbugsPublisher(), junitPublisher(ignoreAttachments: false)]) {
          sh 'mvn clean findbugs:findbugs package'
        }
      }
      post {
        success {
          // we only worry about archiving the jar file if the build steps are successful
         // archiveArtifacts(artifacts: '**/target/*.jar', allowEmptyArchive: true)
        }
      }
    }
	

	
  }

  // All Stages and Pipeline can each have their own post section that is executed at different times
  post {
    always {
		 cleanWs()
      echo "Pipeline is done"
    }
  }
}
