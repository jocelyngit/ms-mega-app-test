node {
    stage ('New Build') {
        
        docker.image(maven:3-alpine).inside {
            git 'https://github.com/jocelyngit/ms-mega-app-test.git'
            
            sh 'mvn -B -DskipTests clean package'
        }
        
    }
}
