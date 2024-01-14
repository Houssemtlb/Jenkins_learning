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

    }


}