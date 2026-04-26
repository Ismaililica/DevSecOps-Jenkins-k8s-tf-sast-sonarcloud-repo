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
		   sh('kubectl delete deployment buggy-deployment -n devsecops || true')
		   sh('kubectl delete service buggy-service -n devsecops || true')
		   sh('kubectl apply -f deployment.yaml --namespace=devsecops')
 
		 }
  
	  }
	}
    
    stage ( ' Wait for testing'){

       steps{

		   sh 'pwd; sleep 180; echo "Application has been deployed on K8S"'
 
	   }

	}
    stage('Run DAST Using Zap') {
    steps {
        // Burası çok kritik: Jenkins'in EKS cluster'ına bağlanabilmesi için bu blok şart!
        withKubeConfig([credentialsId: 'kubelogin']) { 
            script {
                // 1. Dinamik URL'i cluster içinden çekiyoruz
                def app_url = sh(script: "kubectl get service buggy-service -n devsecops -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'", returnStdout: true).trim()
                
                echo "Hedef URL: http://${app_url}"
                
                // 2. Klasör yetkisi
                sh 'chmod 777 ${WORKSPACE}'
                
                // 3. Docker ile tarama (Triple quotes kullanarak değişkeni içeri veriyoruz)
                sh """
                    docker run --rm -v ${WORKSPACE}:/zap/wrk/:rw -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py \
                    -t http://${app_url} \
                    -r zap_report.html || true
                """
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'zap_report.html'
        }
    }
}
}
}
