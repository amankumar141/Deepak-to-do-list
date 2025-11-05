pipeline {
    agent any

    environment {
        APP_NAME = "todo_app"
        PYTHON_ENV = "${WORKSPACE}/venv"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "📦 Checking out code..."
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                echo "🐍 Setting up virtualenv..."
                sh '''
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo "🧪 Running tests..."
                // Add your test script here if available
                sh '''
                    source venv/bin/activate
                    echo "No tests yet, skipping..."
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                echo "🚀 Starting Flask app..."
                sh '''
                    source venv/bin/activate
                    nohup python app.py &
                '''
            }
        }

        stage('Optional: Docker Build') {
            when {
                expression { fileExists('Dockerfile') }
            }
            steps {
                echo "🐳 Building Docker image..."
                sh '''
                    docker build -t flask-todo .
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed."
        }
    }
}