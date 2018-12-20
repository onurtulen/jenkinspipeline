pipeline {
    agent any
    tools { 
        maven 'localMaven' 
        jdk 'localJDK' 
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.224.7.212', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.15.164.24', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /Users/Shared/Jenkins/Home/tomcat-demo.pem /Users/Shared/Jenkins/Home/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/Shared/Jenkins/Home/tomcat-demo.pem /Users/Shared/Jenkins/Home/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
        
    }
}