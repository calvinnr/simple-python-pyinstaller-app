node {
    stage('Checkout') {
        // Checkout your Git repository
        checkout scm
    }

    stage('Install Dependencies') {
        // Use a virtual environment for isolation
        docker.image 'python:3-alpine'
        sh 'python3 -m venv venv'
        sh 'source venv/bin/activate'
        sh 'pip install -r requirements.txt'
    }

    stage('Run Tests') {
        sh 'python3 tests.py'
    }

    stage('Build') {
        sh 'python3 build.py'
    }
}
