pipeline {
  agent any
  tools { 
        maven 'Apache Maven 3.8.4'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=ismaililica-buggy-web-app_ismaililica-buggy-web-app -Dsonar.organization=ismaililica-buggy-web-app -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=4e0cd550c0ee35b89bd63d5ba88ee5b5c367e4be '
			}
        } 

	stage('Run SCA Using SNYK'){
       steps {

		   withCredentials([string(credentialsId: 'SNYK_TOKEN' , variable: 'SNYK_TOKEN')]) {
              sh ' mvn snyk:test -fn '
		   }
	}
  }

	stage('Build'){
      steps{
        withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
            script{
				app= docker.build("ismaililica")

			}
		}
	  }
	}

	stage ('Push') {
     steps {
         script{
              docker.withRegistry('https://624914081331.dkr.ecr.eu-north-1.amazonaws.com', 'ecr:eu-north-1:aws-credentials'){

				  app.push("latest")
			  }

		 }
	 }
	}
    stage ('Kubernetes Deployment of Buggy Web Application'){
      steps {
         withKubeConfig([credentialsId: 'kubelogin']){
           sh('kubectl delete all -all -n devsecops')
		   sh('kubectl apply -f deployment.yaml --namespace=devsecops')
 
		 }
  
	  }
	}
}
}
