
pipeline {
    agent any

    parameters {
         string(name: 'tomcat_prod', defaultValue: '18.218.29.210', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            tools {
                jdk "localJDK"
            }
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

        stage ("Deploy to Production"){
            steps {
                sh "scp -i /var/lib/jenkins/JenkinsTraining.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
            }
        }

      }
}
