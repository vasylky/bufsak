pipeline {
    agent any  
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vasylky/bufsak.git'
            }
        }
        
        stage('Setup Node') {
            steps {
                script {
                    sh 'node --version || (echo "Node.js not found. Please install Node.js on this Jenkins agent" && exit 1)'
                    sh 'npm --version || (echo "npm not found. Please install npm on this Jenkins agent" && exit 1)'
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Check if Docker is available before attempting Docker commands
                    def dockerInstalled = sh(script: 'which docker', returnStatus: true) == 0
                    
                    if (dockerInstalled) {
                        sh 'docker build -t mywebapp .'
                        sh 'docker run -d -p 3000:3000 mywebapp'
                    } else {
                        echo "Docker not found. Skipping Docker deployment steps."
                        echo "To enable Docker deployment, please install Docker on this Jenkins agent."
                        // You could add alternative deployment steps here
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}