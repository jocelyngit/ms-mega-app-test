pipeline {
    agent none

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
    }
}
