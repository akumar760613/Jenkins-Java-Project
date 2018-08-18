pipeline {
  agent none
  
stages {
  stage ('Build') {
    agent {
        label 'master'
      }
  steps {
   echo "BUILD_NUMBER:$BUILD_NUMBER"
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
      sh "java -jar rectangle.jar 3 4"
    }
  }
  stage ('Completed') {
    agent {
        label 'master'
      }
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
