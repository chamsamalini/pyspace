pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'python -m pip install --upgrade pip'
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'python -m pytest -v'
            }
        }

        stage('Package') {
            steps {
                bat 'powershell Compress-Archive -Path app.py,templates,tests,requirements.txt,README.md -DestinationPath sdd-python-webapp.zip -Force'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'sdd-python-webapp.zip', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build successful. The SDD Python web app passed tests and was packaged.'
        }
        failure {
            echo 'Build failed. Check the console output and fix the failing stage.'
        }
    }
}
