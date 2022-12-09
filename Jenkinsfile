pipeline {
  agent any
  tools { 
        maven 'maven_3.5.2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=newbuggywebapp -Dsonar.organization=buggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=8ea8762ea3bef1899ddac2d1ac6d059953b7e2a9'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerhub", url: ""]) {
                 script{
                 app =  docker.build("secdevops")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://297621708399.dkr.ecr.ap-south-1.amazonaws.com/secdevops', 'ecr:ap-south-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
