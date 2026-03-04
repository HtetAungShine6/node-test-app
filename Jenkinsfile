pipeline {
    agent any

    tools {
		nodejs 'NodeJS'
	}

    stages {

        stage('Checkout Github') {
            steps {
                git credentialsId: 'jen-doc-git', url: 'https://github.com/HtetAungShine6/node-test-app.git'
            }
        }

        stage('Install System Dependencies') {
            steps {
                sh 'apt-get update && apt-get install -y libatomic1'
            }
        }

        stage('Install Node Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test Code') {
            steps {
                sh 'npm test'
            }
        }
    }

	post {
		success {
			echo 'Build successful!'
		}
		failure {
			echo 'Build failed. Please check the logs for details.'
		}
	}
}