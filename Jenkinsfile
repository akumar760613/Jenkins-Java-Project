pipeline {
  agent {
    label 'master'
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
   sh "cp dist/rectangle.jar /var/www/html/rectangles/all"
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
