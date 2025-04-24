pipeline {
    agent any
    
    environment {
        APP_DIR = '/root/express-run'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                git 'https://github.com/expressjs/express.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                sh 'npm install'
            }
        }

        stage('Archive Build') {
            steps {
                echo "Archiving build artifact..."
                // Spakowanie aplikacji
                sh 'tar -czf express.tar.gz .'
                echo "Artifact published to workspace: express.tar.gz"
                archiveArtifacts artifacts: 'express.tar.gz', allowEmptyArchive: true
            }
        }

        stage('Start Server') {
            steps {
                echo "🚀 Starting the server..."
                script {
                    // Tworzymy katalog do uruchomienia aplikacji
                    sh 'mkdir -p /root/express-run'
                    // Rozpakowanie artefaktu
                    sh 'tar -xzf express.tar.gz -C /root/express-run'
                    // Przechodzimy do katalogu aplikacji
                    sh 'cd /root/express-run && npm start'
                }
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the application..."
                // Twoje kroki wdrożeniowe
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
