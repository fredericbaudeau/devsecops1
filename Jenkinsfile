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
                                                post {
                                                        always {
                                                                 junit 'target/surefire-reports/*.xml'
                                                                 jacoco execPattern: 'target/jacoco.exec'
                                                                }
                                                     }

                                                 }
//--------------------------
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
//------------------
	   stage('Docker Build and Push') {
      steps {
        withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_HUB_PASSWORD')]) {
          sh 'sudo docker login -u fredericbaudeau -p $DOCKER_HUB_PASSWORD'
          sh 'printenv'
          sh 'sudo docker build -t fredericbaudeau/devsecops1:""$GIT_COMMIT"" .'
          sh 'sudo docker push fredericbaudeau/devsecops1:""$GIT_COMMIT""'
        }

	   
	   }
          }
