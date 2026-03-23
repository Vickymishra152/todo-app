pipeline {
    agent any

    environment {
        IMAGE_NAME = "todo-app"
        CONTAINER_NAME = "todo-app-container"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/Vickymishra152/todo-app.git'
            }
        }

        stage('Build App') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test App') {
            steps {
                sh 'npm test || echo "No tests found"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d -p 3000:3000 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
