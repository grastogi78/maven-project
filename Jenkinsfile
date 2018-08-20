pipeline {
    agent any
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
        stage ('Deploy to Staging'){
            steps {
                build job: 'maven-deployment'
            }

        }
    }
}