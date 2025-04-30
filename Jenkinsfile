pipeline {
    agent any
    
    environment {
        NODE_HOME = '/usr/local/bin/node'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vasylky/bufsak.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker build -t mywebapp .'
                    sh 'docker run -d -p 3000:3000 reactapp'
                }
            }
        }
    }
}
