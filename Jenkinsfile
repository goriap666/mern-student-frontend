pipeline {
  agent any
  triggers { pollSCM('H/2 * * * *') }
  environment {
    CI = 'false'                    // stops CRA treating warnings as errors
    VITE_API_URL = '/api'
    REACT_APP_API_URL = '/api'
  }
  stages {
    stage('Build') {
      steps { sh 'npm ci && npm run build' }
    }
    stage('Deploy') {
      steps {
        sshagent(['deploy-key']) {
          sh '''
            OUT=$( [ -d dist ] && echo dist || echo build )
            rsync -az --delete -e "ssh -o StrictHostKeyChecking=no" $OUT/ ubuntu@10.0.1.30:/var/www/app/
          '''
        }
      }
    }
  }
}
