pipeline {
  agent any
  tools { 
        maven 'Apache Maven 3.8.4'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=ismaililica-buggy-web-app_ismaililica-buggy-web-app -Dsonar.organization=ismaililica-buggy-web-app -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=4e0cd550c0ee35b89bd63d5ba88ee5b5c367e4be
			}
        } 
  }
}
