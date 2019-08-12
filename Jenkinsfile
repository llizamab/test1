pipeline {
  agent {
    label "jenkins-maven"
  }
  environment {
    ORG = 'apside'
    APP_NAME = 'test1'
  }
  stages {
    stage('Pull Request') {
      when {
        branch 'PR-*'
      }
      environment {
        PREVIEW_VERSION = "0.0.0-SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER"
      }
      steps {
        container('maven') {
          sh "echo 'on PR-*...' - version $PREVIEW_VERSION"
          sh "mvn versions:set -DnewVersion=$PREVIEW_VERSION"
          sh "mvn install"
        }
      }
    }
    stage('Deploy QA') {
      when {
        branch 'develop'
      }
      steps {
        sh "echo .. deploying qa"
      }
    }
    stage('Build Release') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          // ensure we're not on a detached head
          sh "git checkout master"
          sh "git config --global credential.helper store"
          sh "echo 'on master...'"
        }
      }
    }
  }
}
