pipeline {
    agent any
    
    tools {
        nodejs 'nodejs'  // same name as configured in Jenkins
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
                sh 'npm start'
            }
        }
    }
}
