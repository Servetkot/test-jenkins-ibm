pipeline {
  agent any
  stages {
    stage('hello jenkins') {
      agent {
        docker {
          image 'rcms_image_with_ftp3'
        }

      }
      environment {
        first = '1'
        second = '2'
      }
      steps {
        echo 'hello'
        sh '''echo "hello world"
echo "hello variable in ocean $first"
echo "hello variable in ocean $second"'''
        input(message: 'Want to continue?', ok: 'yea')
      }
    }
    stage('end jenkins') {
      steps {
        sh 'echo "goodbye"'
        input(message: 'want to end?', ok: 'yeea')
      }
    }
  }
}