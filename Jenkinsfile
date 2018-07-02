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

    stage('deploy-apache') {
      agent {
        label 'master'
      }

      steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangle/all/"
      }

    }

    stage('deploy-tomcat') {
      agent {
        label 'master'
      }

      steps {
        sh "scp dist/rectangle_${env.BUILD_NUMBER}.jar jenkins@ec2-54-209-254-199.compute-1.amazonaws.com:/usr/local/tomcat/apache-tomcat-8.0.52/webapps"
      }

    }

    stage('deploy on slave-FT') {
      agent {
        label 'slave1'
      }

      steps {
        sh "wget http://ec2-34-205-127-163.compute-1.amazonaws.com/rectangle/all/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 5 6"
      }
    }

    stage("Test on Ubuntu") {
      agent {
        docker {
              image 'myubuntu:latest'
              label 'docker'
          }
      }
      steps {
        sh "wget http://ec2-34-205-127-163.compute-1.amazonaws.com/rectangle/all/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 2 4"
      }
    }

  }

}
