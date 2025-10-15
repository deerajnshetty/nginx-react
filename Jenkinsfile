pipeline {
    agent any
    
    tools {
        nodejs 'nodejs'  // same name as configured in Jenkins
    }
    environment {
        EC2_HOST = "ubuntu@52.77.229.122"
        SSH_KEY = "ec2-ssh-key"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git credentialsId: 'github-cred', url: 'https://github.com/deerajnshetty/nginx-react.git', branch: 'master'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

             stage('Deploy to EC2') {
            steps {
                sshagent([env.SSH_KEY]) {
                  sh '''
                       ssh -o StrictHostKeyChecking=no ubuntu@52.77.229.122 "sudo mkdir -p /var/www/react-app && sudo chown -R ubuntu:ubuntu /var/www/react-app"
                       scp -o StrictHostKeyChecking=no -r build/* ubuntu@52.77.229.122:/var/www/react-app/
                       ssh -o StrictHostKeyChecking=no ubuntu@52.77.229.122 "sudo systemctl reload nginx"
                 '''
                }
            }
        }
    }
}
