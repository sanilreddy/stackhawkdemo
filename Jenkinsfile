pipeline {
  agent any
  stages {
    stage ("Checkout code") {
      steps {
        checkout scm
      }
    }
    stage("Deploy site") {
      steps {
        sh 'pwd'
        sh 'cp index.html /var/www/html'
      }
    }
    stage ("Pull HawkScan Image") {
      steps {
        sh 'docker pull stackhawk/hawkscan'
        sh 'echo "${WORKSPACE}"'
      }
    }
    stage ("Run HawkScan Test") {
      environment {
        HAWK_API_KEY = credentials('HAWK_API_KEY')
      }
      steps {
        sh '''
          docker run --rm -v ${WORKSPACE}:/hawk:rw \
            -e API_KEY=${HAWK_API_KEY} -t \
            stackhawk/hawkscan:latest
        '''
      }
    }
  }
}
