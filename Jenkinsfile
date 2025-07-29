node {
    def appDir = "/var/www/next-app"

    stage('Cleanup') {
        echo 'Cleaning up workspace...'
        deleteDir()
    }

    stage('Checkout') {
        git branch: 'main', url: 'https://github.com/yogendragit/jenkins-ci-cd'
    }

    stage('Test') {
        echo 'No tests to run.'
    }

    stage('Deploy') {
        echo 'Deploying Next.js app...'
        sh """
            # Ensure target directory
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            # Copy full source code
            rsync -av --delete ./ ${appDir}/

            # Build and run app
            cd ${appDir}
            npm install
            npm run build

            # Stop old app
            pm2 stop next-app || true

            # Start app
            pm2 start npm --name "next-app" -- run start -- -H 0.0.0.0 -p 3000
            pm2 save
        """
    }
}
