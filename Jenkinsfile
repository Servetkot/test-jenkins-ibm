pipeline {
  agent any
  stages {
    stage('hello jenkins') {
      steps {
        echo 'hello'
        sh 'echo "hello world"'
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