pipeline {
  agent none
  
stages {
  stage ('Build') {
    agent {
        label 'master'
      }
  steps {
    sh 'ant -f build.xml -v'
   }
    post {
    success {
      archiveArtifacts artifacts: 'dist/', fingerprint: true
      }
    }
 }
 stage ('Unit Testing') {
   agent {
        label 'master'
      }
    steps {
      sh 'ant -f test.xml -v'
      junit 'reports/result.xml'
    }
 }
 stage ('Deploy') {
   agent {
        label 'master'
      }
 steps {
   sh "cp dist/rectangle.jar /var/www/html/rectangles/all"
      }
    }
  stage ("Running on Centos") {
    agent {
      label 'master'
    }
    steps {
      sh "wget http://jsudepally1.mylabserver.com/rectangles/all/rectangle.jar"
      echo "Successfully downloaded jar file and updated with build number"
    }
  }
  stage ('Completed') {
    agent {
        label 'master'
      }
  steps {
    echo "Successfully Deployed"
   }
  }
}
}
