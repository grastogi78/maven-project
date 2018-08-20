pipeline {
    agent any

    parameters {
         string(name: 'tomcat_staging', defaultValue: '54.225.56.224', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.201.52.55', description: 'Production Server')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /var/lib/jenkins/JenkinsTraining.pem -o StrictHostKeyChecking=no **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /var/lib/jenkins/JenkinsTraining.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}
