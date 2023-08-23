pipeline {
	agent any
	stages {
//----BUILD
		stage('Build Artifact') {
                        steps {
                               sh "mvn clean package -DskipTests=true"
                               archive 'target/*.jar'  //test
                               }
                                         }   
//----JACOCO
	        stage('UNIT test & jacoco ') {
                        steps {
                               sh "mvn test"
                               }
                        post  {
                               always {
                                   junit 'target/surefire-reports/*.xml'
                                   jacoco execPattern: 'target/jacoco.exec'
                                      }
                               }

                                              }
//-----MUTATION
		stage('Mutation Tests - PIT') {
                	steps {
                        	sh "mvn org.pitest:pitest-maven:mutationCoverage"
                              }
			post { 
                        	always { 
                                	pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
                                        }
                             }
                                               }
//-----DOCKER
		stage('Docker Build and Push') {
			steps {
		                withCredentials([string(credentialsId: 'Freddock', variable: 'FREDDOCK')]) {
				          sh 'sudo docker login -u fred3945 -p $FREDDOCK'
				          sh 'printenv'
				          sh 'sudo docker build -t fred3945/devsecops1:""$GIT_COMMIT"" .'
				          sh 'sudo docker push fred3945/devsecops1:""$GIT_COMMIT""'
		        	                                  					    }
		             }
         					}
//-----KUBERNETES	
		stage('Deployment Kubernetes  ') {
      			steps {
        			withKubeConfig([credentialsId: 'idkuberfred']) {
               			sh "sed -i 's#replace#fredericbaudeau/devops-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
               			sh "kubectl apply -f k8s_deployment_service.yaml"
             		     							}															
      						}
    				}    
	
//------------------------------------------------------------------------------------------------------------------------		
   }
}
