node {
    def appDir = "/var/www/next-app"
    stage('Cleanup') {
        echo 'Cleaning up...'
        deleteDir()
    }
    stage('Checkout from git') {
        git branch: 'main', url: 'https://github.com/yogendragit/jenkins-ci-cd'
    }
    stage('Test') {
        echo 'No tests to run.'
    }

    stage('Build') {
        echo 'Installing dependencies and building app...'
        sh '''
            npm install
            npm run build
        '''
    }

    stage('Deploy') {
        echo 'Deploying...'
        sh """
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            # Copy built files to deployment directory
            rsync -av --delete .next/ ${appDir}/.next/
            rsync -av --delete public/ ${appDir}/public/
            if [ -f next.config.js ]; then
                rsync -av next.config.js /var/www/next-app/
            fi

            # Stop existing process on port 3000 if any
            sudo fuser -k 3000/tcp || true

            # Start app in background (consider using PM2 for production)
            cd ${appDir}
            nohup npm run start > app.log 2>&1 &
        """
    }
}
