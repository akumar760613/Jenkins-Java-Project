 pipeline {
  agent none
  
  stages {  
   stage('Say Hello') {
    agent {
       label 'master'
    }
    steps {
      sayHello'Good Work Arun!'
    }
  }
     stage('Unit Tests') {
      agent {
        label 'master'
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build') {
      agent {
        label 'master'
      }
      steps {
        sh 'ant -f build.xml -v'
      }
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
    }
    stage('deploy') {
      agent {
        label 'master'
      }
      steps {
        sh "cp dist/rectangle.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
      }
    }
    stage("Running on CentOS") {
      agent {
        label 'master'
      }
      steps {
        sh "wget http://jsudepally1.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle.jar"
         echo "Successfully Downloaded jar file"
      }
    }
    stage("Test on Debian") {
      agent {
        docker 'openjdk:8u121-jre'
      }
      steps {
        sh "wget http://jsudepally1.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle.jar"
        echo "Successfully Downloaded jar file"
      }
    }
    stage('Promote to Green') {
      agent {
        label 'master'
      }
      steps {
        sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/rectangle.jar /var/www/html/rectangles/green/rectangle.jar"
      }
    }
    stage('Promote Development Branch to Master') {
      agent {
        label 'master'
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
        echo 'Tagging the Release'
        sh "git tag rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
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
 }
