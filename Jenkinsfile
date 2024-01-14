pipeline {
      agent any
      stages {
          stage("Tests"){
              steps{
                bat 'gradlew test'
              }
              post{
                success{
                  archiveArtifacts 'target/*.json'
                }
              }
          }
          stage('Test Reporting') {
              steps {
                cucumber 'reports/*json'
              }
          }
          stage('Code Quality & Analysis') {
                      steps {
                        withSonarQubeEnv('sonar') {
                          bat(script: 'gradlew sonarqube', returnStatus: true)
                        }
                        waitForQualityGate true
                      }
          }

    }


}