pipeline {
    agent any
    
    tools {
        nodejs 'nodejs'  // same name as configured in Jenkins
    }

    environment {
        DEPLOY_USER = 'ubuntu'
        DEPLOY_HOST = '18.141.9.218'
        DEPLOY_PATH = '/var/www/html'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    credentialsId: 'github-cred', // Replace with your Jenkins GitHub credential ID
                    url: 'https://github.com/deerajnshetty/nginx-react.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to Remote NGINX Server') {
            steps {
                sshagent (credentials: ['ec2-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_HOST} 'sudo chown -R ubuntu:ubuntu /var/www/html'
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_HOST} 'sudo rm -rf ${DEPLOY_PATH}/*'
                        scp -r -o StrictHostKeyChecking=no build/* ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}/
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_HOST} 'sudo systemctl reload nginx'
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! React app is live on NGINX server.'
        }
        failure {
            echo '❌ Deployment failed. Check logs for details.'
        }
    }
}
