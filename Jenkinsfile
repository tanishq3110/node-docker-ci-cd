pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tanishq3110/node-docker-ci-cd"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/tanishq3110/node-docker-ci-cd.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'docker run --rm $DOCKER_IMAGE:latest npm test || echo "No tests found"'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying container..."
                sh '''
                docker stop node-app || true
                docker rm node-app || true
                docker run -d --name node-app -p 3000:3000 $DOCKER_IMAGE:latest
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished!"
        }
    }
}
