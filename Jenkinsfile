pipeline {
	agent any
	tools {
		nodejs 'node20'
	}
	stages {
		stage('Checkout Github'){
			steps {
				git branch: 'master', credentialsId: 'e7458a56-fc23-4da6-a076-467f6203cdf2', url: 'https://github.com/HtetAungShine6/simple-node-js-react-npm-app.git'
			}
		}		
		stage('Install node dependencies'){
			steps {
				sh 'npm install'
			}
		}
		stage('Test Code'){
			steps {
				sh 'npm test'
			}
		}
	}

	post {
		success {
			echo 'Build&Deploy completed succesfully!'
		}
		failure {
			echo 'Build&Deploy failed. Check logs.'
		}
	}
}