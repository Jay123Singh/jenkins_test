pipeline {
	agent any
	//triggers {
		//pollSCM '*/2 * * * *'
	//}

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
		jdk "jdk1.8"
    }

    stages {
	   stages {
	    stage('enable webhook'){
	  	    steps {
			    script { properties([pipelineTriggers([githubPush()])])
				   }
		    }
	    }
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
				git credentialsId: 'github', url: 'git@github.com:Jay123Singh/jenkins_test.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway clean package"

                // To run Maven on a Windows agent, use
                //bat "mvn -Dmaven.test.failure.ignore=true -f api-gateway clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    //junit '**/target/surefire-reports/*.xml'
                    junit 'api-gateway/target/surefire-reports/*.xml'
					archiveArtifacts 'api-gateway/target/*.jar'
                }
            }
        }
    }
}
