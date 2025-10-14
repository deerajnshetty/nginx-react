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

        stage('Serve App') {
          steps {
            sshagent([env.SSH_KEY])
              sh 'npm start'
            }
        }
    }
}
