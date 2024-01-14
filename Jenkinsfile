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
          stage('Notification') {
              parallel{
                stage('Email Notification'){
                    steps {
                        mail(subject: 'Deployment success', body: mail, cc: 'kh_talbi@esi.dz', bcc: 'kf_zemmouri@esi.dz')
                     }
                 }
                 stage('others Notification'){
                     steps{
                         notifyEvents message: mail, token: '6veabcpemgadf_0xrfpb7gfbxozhw49j'
                     }
                }
              }

          }

    }


}