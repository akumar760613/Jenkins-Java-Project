pipeline {
  agent any

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
   sh 'ant -f build.xml -v'
      }
    }
  }
}
