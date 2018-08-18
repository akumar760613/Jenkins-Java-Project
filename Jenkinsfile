pipeline {
  agent any
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1' ))
  }
stages {
  stage ('Build') {
  steps {
   echo "Building"
   }
 }
 stage ('Unit Testing') {
    steps {
      sh 'ant -f test.xml -v'
      junit 'reports/result.xml'
    }
 }
 stage ('Deploy') {
 steps {
   echo "Deploying"
      }
    }
  stage ('Build_number') {
  steps {
    echo "JOB_NAME: ${env.BUILD_NUMBER}"
   }
  post {
    always {
      archiveArtifacts artifacts: 'dist/', fingerprint: true
      }
    }
  }
}
}
