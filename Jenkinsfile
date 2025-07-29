node {
    def appDir = "/var/www/next-app"
    stage('Cleanup') {
        echo 'Cleaning up...'
        deleteDir()
    }
    stage('Checkout from git') {
        // Clone the repo
        git branch: 'main', url: 'https://github.com/yogendragit/jenkins-ci-cd'
    }
    stage('Test') {
        // Optional: add tests if available
        // sh 'npm test'
        echo 'No tests to run.'
    }

    stage('Deploy') {
        echo 'Deploying...'
        sh """
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}
            
           # Sync build output and assets
            if [ -d ".next" ]; then
                rsync -av .next/ /var/www/next-app/.next/
                else
                echo "Build folder .next not found"
                exit 1
            fi
            rsync -av --delete public/ /var/www/next-app/public/
            rsync -av package.json next.config.js /var/www/next-app/
            --exclude='.git'
            --exclude='node_modules' ./${appDir}
            cd ${appDir}
            sudo npm install
            sudo npm run build
            sudo fuser -k 3000/tcp || true
            npm run start

        """
    }

    
}