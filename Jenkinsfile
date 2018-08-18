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
 stage ('test') {
 steps {
  echo "Testing"
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
  }
   stage ('Unit Testing') {
    steps {
      sh 'ant -f test.xml -v'
      junit 'reports/result.xml'
    }
  post {
    always {
      archiveArtifacts artifacts: 'dist/', fingerprint: true
      }
    }
  }
}
}
