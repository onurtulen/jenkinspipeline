pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh '${MAVEN_HOME}/bin/mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}