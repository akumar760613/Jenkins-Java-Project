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
   sh "cp dist/rectangle.jar /var/www/html/rectangles/all/"
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
   stage("Test on Debian") {
      agent {
        docker 'openjdk:8u121-jre'
      }
      steps {
      sh "wget http://jsudepally1.mylabserver.com/rectangles/all/rectangle.jar"
      echo "Successfully downloaded jar file and updated with build number"
    }
  }
  stage ('Promote to Green') {
    agent {
      label 'master'
    }
   when {
        branch 'master'
      }
  steps {
   sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
   }
 }
 stage('Promote Development Branch to Master') {
      agent {
        label 'master'
      }
      when {
        branch 'development'
      }
      steps {
        echo "Stashing Any Local Changes"
        sh 'git stash'
        echo "Checking Out Development Branch"
        sh 'git checkout development'
        echo 'Checking Out Master Branch'
        sh 'git pull origin'
        sh 'git checkout master'
        echo 'Merging Development into Master Branch'
        sh 'git merge development'
        echo 'Pushing to Origin Master'
        sh 'git push origin master'
        echo 'Tagging the Release'
        sh "git tag rectangle-${env.BUILD_NUMBER}"
        sh "git push origin rectangle-${env.BUILD_NUMBER}"
      }
      post {
        success {
          emailext(
            subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Development Promoted to Master",
            body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Development Promoted to Master":</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
            to: "arun.immadi8@gmail.com"
          )
        }
      }
    }
  }
  post {
    failure {
      emailext(
        subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Failed!",
        body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Failed!":</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
         to: "arun.immadi8@gmail.com"
      )
    }
  }
  stage('Completed') {
    agent {
        label 'master'
      }
  steps {
    echo "Successfully Deployed"
   }
  }
}
