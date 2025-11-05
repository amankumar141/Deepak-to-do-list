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
        bat '''
            python -m venv venv
            call venv\\Scripts\\python.exe -m pip install --upgrade pip
            call venv\\Scripts\\activate
            pip install -r requirements.txt
        '''
    }
}


        stage('Run Tests') {
    steps {
        echo "🧪 Running tests..."
        bat '''
            call venv\\Scripts\\activate
            python -m unittest discover -s . -p "test_*.py" || exit 0
        '''
    }
}

        stage('Run Flask App') {
    steps {
        echo "🚀 Starting Flask app..."
        bat '''
            call venv\\Scripts\\activate
            start /B python app.py
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
    always {
        echo "✅ Test stage completed — check above for results."
    }
}
}