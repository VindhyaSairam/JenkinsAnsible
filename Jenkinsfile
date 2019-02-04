pipeline {

  agent none

  stages {


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
   
    stage('test using ansible') {
      agent {
        label 'master'
      }
      steps {
        sh 'pwd'
        

      }
    }


  }

}
