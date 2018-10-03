pipeline {
  agent {
    label 'liw'
  }
  stages {
    stage('Preparation') {
      steps {
        sh 'echo "preparation"'
      }
    }
    stage('Build in E') {
      steps {
        withCredentials(bindings: [[$class: 'UsernamePasswordMultiBinding', credentialsId:
                'meggp.vipa.uk.ibm.com', usernameVariable: 'mysecretusername',
                passwordVariable: 'mysecretpassword']]) {
          sh '''export JOBID=$(bash uploadjob.sh 
$mysecretusername $mysecretpassword 
jcl/compilee.jcl) && 
bash getjobresult.sh 
$mysecretusername $mysecretpassword $JOBID 
&& bash parsejob.sh $JOBID'''
        }

      }
    }
    stage('Running tests in E') {
      steps {
        withCredentials(bindings: [[$class: 'UsernamePasswordMultiBinding', credentialsId:
                'meggp.vipa.uk.ibm.com', usernameVariable: 'mysecretusername',
                passwordVariable: 'mysecretpassword']]) {
          sh '''export JOBID=$(bash uploadjob.sh 
$mysecretusername $mysecretpassword 
jcl/test.jcl) && bash getjobresult.sh 
$mysecretusername $mysecretpassword 
$JOBID && bash parsetestjob.sh $JOBID 
&& bash downloadtestfile.sh 
$mysecretusername $mysecretpassword RTC.LIWXBTH.OTST'''
        }

        sh 'python generate_xunit.py'
        sh 'ls -la reports/*'
        junit 'reports/*.xml'
        sh 'bash testresultparser.sh'
      }
    }
    stage('Approve build for Q') {
      steps {
        slackSend(color: "${env.SLACK_COLOR_WARNING}", channel: '#liw-automation', message: "*APPROVAL REQUIRED for promotion from E to Q:* https://txo-liw-jenkins.swg-devops.com/blue/organizations/jenkins/LIW-demo/detail/LIW-demo/${env.BUILD_NUMBER}/pipeline")
        input 'Please approve promotion from E to Q'
        slackSend(color: "${env.SLACK_COLOR_GOOD}", channel: '#liw-automation', message: "*APPROVED promotion ${env.BUILD_NUMBER} from E to Q by ${env.USER_ID}")
      }
    }
    stage('Build in Q') {
      steps {
        sh 'echo "Building in Q"'
      }
    }
    stage('Running tests in Q') {
      steps {
        sh 'echo "Running tests in Q"'
      }
    }
    stage('Approve build for T') {
      steps {
        slackSend(color: "${env.SLACK_COLOR_WARNING}", channel: '#liw-automation', message: "*APPROVAL REQUIRED for promotion from Q to T:* https://txo-liw-jenkins.swg-devops.com/blue/organizations/jenkins/LIW-demo/detail/LIW-demo/${env.BUILD_NUMBER}/pipeline")
        input 'Please approve promotion from Q to T'
        slackSend(color: "${env.SLACK_COLOR_GOOD}", channel: '#liw-automation', message: "*APPROVED promotion ${env.BUILD_NUMBER} from Q to T by ${env.USER_ID}")
      }
    }
    stage('Build in T') {
      steps {
        sh 'echo "Building in T"'
      }
    }
    stage('Running tests in T') {
      steps {
        sh 'echo "Running tests in T"'
      }
    }
    stage('Approve build for T.LIVE') {
      steps {
        slackSend(color: "${env.SLACK_COLOR_WARNING}", channel: '#test-rcms-devops', message: "*APPROVAL REQUIRED for promotion from T to T.LIVE:* https://txo-liw-jenkins.swg-devops.com/blue/organizations/jenkins/LIW-demo/detail/LIW-demo/${env.BUILD_NUMBER}/pipeline")
        input 'Please approve promotion from T to T.LIVE'
        slackSend(color: "${env.SLACK_COLOR_GOOD}", channel: '#liw-automation', message: "*APPROVED promotion ${env.BUILD_NUMBER} from T to T.LIVE by ${env.USER_ID}")
      }
    }
    stage('Build in T.LIVE') {
      steps {
        sh 'echo "Building in T.LIVE"'
      }
    }
    stage('Running tests in T.LIVE') {
      steps {
        sh 'echo "Running tests in T.LIVE"'
      }
    }
    stage('Approve build for P') {
      steps {
        slackSend(color: "${env.SLACK_COLOR_WARNING}", channel: '#liw-automation', message: "*APPROVAL REQUIRED for promotion from T.LIVE to P :* https://txo-liw-jenkins.swg-devops.com/blue/organizations/jenkins/LIW-demo/detail/LIW-demo/${env.BUILD_NUMBER}/pipeline")
        input 'Please approve promotion from T.LIVE to P'
        slackSend(color: "${env.SLACK_COLOR_GOOD}", channel: '#liw-automation', message: "*APPROVED promotion ${env.BUILD_NUMBER} from T.LIVE to P by ${env.USER_ID}")
      }
    }
    stage('Build in P') {
      steps {
        sh 'echo "Building in P"'
      }
    }
    stage('Running tests in P') {
      steps {
        sh 'echo "Running tests in P"'
      }
    }
  }
  environment {
    SLACK_COLOR_DANGER = '#E01563'
    SLACK_COLOR_INFO = '#6ECADC'
    SLACK_COLOR_WARNING = '#FFC300'
    SLACK_COLOR_GOOD = '#3EB991'
  }
  post {
    aborted {
      echo 'Sending message to Slack'
      slackSend(color: "${env.SLACK_COLOR_WARNING}", channel: '#liw-automation', message: "*ABORTED:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${env.USER_ID}\n More info at: ${env.BUILD_URL}")

    }

    failure {
      echo 'Sending message to Slack'
      slackSend(color: "${env.SLACK_COLOR_DANGER}", channel: '#liw-automation', message: "*FAILED:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${env.USER_ID}\n More info at: ${env.BUILD_URL}")

    }

    success {
      echo 'Sending message to Slack'
      slackSend(color: "${env.SLACK_COLOR_GOOD}", channel: '#liw-automation', message: "*SUCCESS:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${env.USER_ID}\n More info at: ${env.BUILD_URL}")

    }

  }
}