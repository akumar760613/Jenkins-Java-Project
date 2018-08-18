pipeline {
  agent {
    label 'master'
  }
stages {
  stage ('Build') {
  steps {
   echo "BUILD_NUMBER:$BUILD_NUMBER"
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
  stage ('Completed') {
  steps {
    echo "Successfully Deployed"
   }
  post {
    always {
      archiveArtifacts artifacts: 'dist/', fingerprint: true
      }
    }
  }
}
}
