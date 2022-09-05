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
          docker run -v ${WORKSPACE}:/hawk:rw -t \
            -e API_KEY=${HAWK_API_KEY} \
            -e NO_COLOR=true \
            stackhawk/hawkscan:2.7.0
        '''
      }
    }
  }
}
