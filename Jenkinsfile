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
//         sh 'cp index.json /var/www/html'
      }
    }
    stage ("Pull HawkScan Image") {
      steps {
        sh 'docker pull stackhawk/hawkscan'
      }
    }
    stage ("Run HawkScan Test") {
      environment {
        HAWK_API_KEY = credentials('stackhawk-api-key')
      }
      steps {
        sh '''
          docker run -v ${WORKSPACE}:/hawk:rw -t \
            -e API_KEY=${HAWK_API_KEY} \
            -e NO_COLOR=true \
            stackhawk/hawkscan
        '''
      }
    }
  }
}
