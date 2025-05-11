pipeline {  
    agent any  
        stages {
			stage('Build') { 
            steps {
                sh 'mvn -B clean package' 
            }
        }
       	    stage("git_checkout") {  
           	    steps {  
              	    echo "cloning repository" 
              	    echo "repo cloned successfully"  
              	    }  
         	    }
				stage('Run Sonarqube') {
            		environment {
                		scannerHome = tool 'lil-sonar-tool';
						}
            	steps {
              		withSonarQubeEnv(credentialsId: 'sonarqube2', installationName: 'lil sonar installation') {
                	sh "${scannerHome}/bin/sonar-scanner"
              	}
            }
		}
			stage('Dependency Check') {
				steps {
        			sh 'mvn verify -Ddependency-check.failBuildOnCVSS=7'
				}
				post {
					always {
						archiveArtifacts artifacts: 'target/dependency-check-report.html', fingerprint: true
					}
				}
			}
        }
}
