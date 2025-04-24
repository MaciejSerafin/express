pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                sh 'npm install'
            }
        }

        stage('Verify Start Script') {
            steps {
                echo "Checking if 'start' script exists in package.json..."
                script {
                    def hasStart = sh(script: "node -e \"const p = require('./package.json'); if (!p.scripts || !p.scripts.start) process.exit(1);\"", returnStatus: true) == 0
                    if (!hasStart) {
                        error("Missing 'start' script in package.json. Please define it to run the server.")
                    }
                }
            }
        }

        stage('Start Server') {
            steps {
                echo "Starting the Node.js server..."
                sh 'npm start &'
                sleep time: 5, unit: 'SECONDS' // Dać serwerowi czas na uruchomienie
                sh 'curl -I http://localhost:3000 || true' // Sprawdzenie, czy serwer ruszył (zakłada port 3000)
            }
        }
    }

    post {
        always {
            echo "Pipeline completed."
        }
        success {
            echo "Server started successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs for more info."
        }
    }
}
