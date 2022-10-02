pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package'
            }
            post {
                success {
                    // One or more steps need to be included within each condition's block.
                    junit 'target/surefire-reports/*xml'
                    archiveArtifacts artifacts: 'target/*jar', followSymlinks: false
                }
            }
        }
        stage('Deploy') {
            steps {
                 withEnv(['JENKINS_NODE_COOKIE=dontKillMe']){
                 sh 'nohup sudo -u jenkins java -Dserver.port=8002 -jar target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &'
                } 
            }
        }
   }
}
