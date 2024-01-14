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
          stage('Code Analysis') {
                      steps {
                        withSonarQubeEnv('sonar') {
                          bat 'gradlew sonar'
                        }
                      }
          }
          stage("Quality Gate") {
                      steps {
                        timeout(time: 1, unit: 'HOURS') {
                          waitForQualityGate abortPipeline: true
                        }
                      }
                  }



          stage('build') {
                steps {
                  bat 'gradlew build'
                  bat 'gradlew javadoc'
                  archiveArtifacts 'build/libs/*.jar'
                  junit(testResults: 'build/test-results/test/*.xml', allowEmptyResults: true)
                }
          }
          stage('Deploy'){
                steps{
                    bat 'gradlew publish'
                }
          }
          stage('Email Notification'){
              steps {
                emailext subject: 'Deployment Successful',
                    body: 'The deployment was successful. You can access the deployed application.',
                    to: 'kf_zemmouri@esi.dz'

               }
          }




    }


}