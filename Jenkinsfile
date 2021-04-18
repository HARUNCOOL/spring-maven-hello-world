pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'mvn package'
      }
    }

    stage('artifact') {
      steps {
        archiveArtifacts 'target/*.war'
      }
    }

  }
}