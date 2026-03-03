pipeline {
    agent any
    tools {
        nodejs 'node25'
    }
    stages {
        stage('Build') {
            steps {
                sh 'node -v'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
}