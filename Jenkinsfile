pipeline {
    agent any
	
	environment {
		def customImage = ''
	}
	
	tools {
		maven 'Maven 3.5.4' 
        	jdk 'jdk8' 
	}

    stages {
	
			stage ('Initialize') {
				steps {
		
					sh '''
						echo "PATH = ${PATH}"
						echo "M2_HOME = ${M2_HOME}"
						echo  "JAVA_HOME = ${JAVA_HOME}"
					''' 
				}
			}
			
			stage('Build') {
				steps {
					 sh 'mvn -B -DskipTests clean package'
                }
            }
			
        stage('Build Docker Image') {
            steps {
                script {
                     customImage = docker.build("megaappimage:${env.BUILD_ID}")
				}
			}
		}
	    
	    stage('push Docker Image') {
            steps {
                script {
			sh "docker tag megaappimage:${env.BUILD_ID} localhost:5000/megaappimage"
			
			sh "docker push localhost:5000/megaappimage"
				}
			}
		}
		
        stage('run docker Image') {
			steps {
				script {
					
					sh "docker run --name msmega --detach --restart=always --publish 8000:8081 --publish 45000:50000 localhost:5000/megaappimage"
				}
			}
		}
		stage ('Deploy to kubernetes') {
			steps {
				script {
					kubernetesDeploy(configs: "msmega.yaml", kubeconfigId: "kubeglobalcredentialid")
				}  
			}
		}
		
    }
}
