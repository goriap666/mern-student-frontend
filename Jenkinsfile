pipeline {
  agent any
  triggers { pollSCM('H/2 * * * *') }
  environment {
    SERVER = 'ubuntu@10.0.1.20'
    SSH    = 'ssh -o StrictHostKeyChecking=no'
  }
  stages {
    stage('Deploy') {
      steps {
        sshagent(['deploy-key']) {
          sh '''
            rsync -az --delete --exclude node_modules --exclude .git --exclude .env \
              -e "$SSH" ./ $SERVER:/home/ubuntu/backend/
            $SSH $SERVER "cd /home/ubuntu/backend && npm ci --omit=dev && pm2 startOrReload ecosystem.config.js --update-env && pm2 save"
          '''
        }
      }
    }
  }
}
