//def notifySuccessful() {
//  slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
//}

pipeline {
  agent any
  stages {
    stage('Clone') {
      steps {
        stash(name: 'scm', includes: '*')
        git(url: 'https://github.com/bliekp/BrouwHulp.git', branch: "${env.BRANCH_NAME}", changelog: true, poll: true)
      }
    }
    stage('Build in Docker') {
      steps {
        unstash 'scm'
        script {
          docker.image('taraworks/lazarus-cross:0.0.2').inside{
            sh 'pwd'
            sh 'ls -altrh'
            sh 'lazbuild brouwhulp.lpi'
            sh 'ls -altrh'
          }
        }

      }
    }
  }
  
      post {
        always {
            archiveArtifacts artifacts: 'hello', fingerprint: true
        }
        success {
//            notifySuccessful()
            echo "SUCCESS!"
        }
      }
}