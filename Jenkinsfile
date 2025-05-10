pipeline {  
    agent any  
        stages {
			stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
       	    stage("git_checkout") {  
           	    steps {  
              	    echo "cloning repository" 
              	    echo "repo cloned successfully"  
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
