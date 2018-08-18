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
   sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all"
      }
    }
  stage ("Running on Centos") {
    agent {
      label 'master'
    }
    steps {
      sh "wget http://jsudepally1.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
      echo "Successfully downloaded jar file and updated with build number"
    }
  }
   stage("Test on Debian") {
      agent {
        docker 'openjdk:8u121-jre'
      }
      steps {
      sh "wget http://jsudepally1.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
      echo "Successfully downloaded jar file and updated with build number"
    }
  }
  stage ('Promote to Green') {
  steps {
   sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
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
