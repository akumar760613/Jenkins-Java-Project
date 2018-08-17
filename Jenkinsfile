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
      }
    }
  stage ('Build_number') {
  steps {
    echo "My Branch Name: ${env.BRANCH_NAME}"
   }
  post {
    always {
      archive 'dist/*.jar'
      }
    }
  }
}
}
