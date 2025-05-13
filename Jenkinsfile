pipeline {  
    agent any  
    stages {
        stage("git_checkout") {  
            steps {  
                echo "cloning repository" 
                echo "repo cloned successfully"  
            }  
        }
        
        stage('Build') { 
            steps {
                sh 'mvn -B clean package' 
            }
        }

        stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'SSLDC-sonarqube';
            }
            steps {
                withSonarQubeEnv(credentialsId: 'sonarqube2', installationName: 'LocalSonar') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.host.url=http://sonarqube2:9000"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
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
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }
        stage('Notify') {
            steps {
                echo "---Pipeline completata con successo."
                echo "---Nessuna vulnerabilità con CVSS >= 7 trovata."
                echo "---Quality Gate SonarQube superato."
                echo "---Artefatto salvato: target/onlinebookstore.war"
            }
        }
    }
}
