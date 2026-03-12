pipeline {
    agent any

    tools {
		nodejs 'NodeJS'
	}

	environment {
		DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
		IMAGE_NAME = "${DOCKERHUB_CREDENTIALS_USR}/nodeimage"
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
		stage('Build Docker Image') {
			steps {
				sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
			}
		}
		stage('Push Docker Image') {
			steps {
				// Create the Docker Hub repo via API (safe to run if repo already exists)
				sh '''
					REPO_NAME=$(echo $IMAGE_NAME | cut -d'/' -f2)
					TOKEN=$(curl -s -X POST "https://hub.docker.com/v2/users/login/" \
						-H "Content-Type: application/json" \
						-d "{\"username\": \"$DOCKERHUB_CREDENTIALS_USR\", \"password\": \"$DOCKERHUB_CREDENTIALS_PSW\"}" \
						| python3 -c "import sys,json; print(json.load(sys.stdin)['token'])")
					curl -s -o /dev/null -X POST "https://hub.docker.com/v2/repositories/" \
						-H "Authorization: JWT $TOKEN" \
						-H "Content-Type: application/json" \
						-d "{\"name\": \"$REPO_NAME\", \"namespace\": \"$DOCKERHUB_CREDENTIALS_USR\", \"is_private\": false}" || true
				'''
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
				sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
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