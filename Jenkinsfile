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
    sh """
        npm install
        npm run build
    """
}

stage('Deploy') {
    echo 'Deploying...'
    sh """
        sudo mkdir -p ${appDir}
        sudo chown -R jenkins:jenkins ${appDir}

        # Copy build output
        rsync -av --delete .next/ ${appDir}/.next/
        rsync -av --delete public/ ${appDir}/public/
        rsync -av package.json ${appDir}/
        rsync -av package-lock.json ${appDir}/
        rsync -av node_modules/ ${appDir}/node_modules/

        # Kill app on port 3000 if any
        sudo fuser -k 3000/tcp || true

        # Run app in background (nohup) as jenkins user
        cd ${appDir}
        nohup npm run start -- -H 0.0.0.0 -p 3000 > app.log 2>&1 &
    """
}


}
