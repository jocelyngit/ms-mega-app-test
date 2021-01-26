pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                     docker {
                        image 'maven:3-alpine'
                        args '-v /root/.m2:/root/.m2'
                            }
                        }
            steps {
                sh 'mvn -B -DskipTests clean package'
                }
            }
        stage('Build Docker Image') {
                steps {
                echo 'Starting to build docker image'
                script {
                    def customImage = docker.build("msmegaappimage:${env.BUILD_ID}")

            }
        }
		}
        stage('run docker Image') {
			steps {
				script {
				echo "-=- run Docker image -=-"
				sh "docker run --name msmega --detach --publish 8081:8081 --publish 45000:45000 msmegaappimage:${env.BUILD_ID}"
				}
				}
		}
		stage ('Deploy to kubernetes') {
			steps {
				script {
					kubernetesDeploy(configs: "kube/deploy.yaml", kubeconfigId: "monkubeconfig")
				}
			}
		}
		
    }
}
