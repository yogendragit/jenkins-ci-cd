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
            cd ${appDir}
            sudo npm install
            sudo npm run build
            sudo fuser -k 3000/tcp || true
            npm run start

        """
    }

    
}