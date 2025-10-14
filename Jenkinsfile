pipeline {
    agent any

    tools {
        nodejs "Node20"  // or whatever NodeJS tool name you configured
    }

    environment {
        EC2_HOST = "ubuntu@54.255.244.33"
        SSH_KEY = "ec2-ssh-key"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/deerajnshetty/nginx-react.git'
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

        stage('Deploy to EC2') {
            steps {
                sshbuildagent([env.SSH_KEY]) {
                    sh """
                    scp -o StrictHostKeyChecking=no -r build/* ${EC2_HOST}:/var/www/react-app/
                    ssh -o StrictHostKeyChecking=no ${EC2_HOST} 'sudo systemctl reload nginx'
                    """
                }
            }
        }
    }
}
