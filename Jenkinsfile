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
        sshagent (credentials: ['jenkins_key']) {
                    ansiblePlaybook(
                       credentialsId: 'jenkins_key', 
                       inventory: '/etc/ansible/hosts', 
                       playbook: '/var/lib/jenkins/playbooks/ec2-test.yml',
                       extraVars: [
                          path: "${env.WORKSPACE}", 
                          BUILD_NUMBER: "${env.BUILD_NUMBER}"
                       ])
        }
      }
    }


  }

}
