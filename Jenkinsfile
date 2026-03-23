pipeline {
    agent any

    environment {
        // You can define environment variables here
        APP_NAME = "todo-app"
        DOCKER_IMAGE = "todo-app:latest"
    }

    stages {

        stage('Checkout SCM') {
            steps {
                // Checkout the branch that triggered the build
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/Vickymishra152/todo-app.git'
            }
        }

        stage('Build App') {
            steps {
                echo "Building ${env.APP_NAME}..."
                // Example: for Node.js
                sh '''
                    cd todo-app
                    npm install
                    npm run build
                '''
            }
        }

        stage('Test App') {
            steps {
                echo "Running tests..."
                sh '''
                    cd todo-app
                    npm test
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image ${env.DOCKER_IMAGE}..."
                sh '''
                    docker build -t ${DOCKER_IMAGE} todo-app/
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                echo "Stopping old container if it exists..."
                sh '''
                    docker stop ${APP_NAME} || true
                    docker rm ${APP_NAME} || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                echo "Running new container..."
                sh '''
                    docker run -d --name ${APP_NAME} -p 3000:3000 ${DOCKER_IMAGE}
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build and deploy successful!"
        }
        failure {
            echo "❌ Build failed!"
        }
    }
}
