pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git 'https://github.com/MaciejSerafin/express.git'
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
                    def hasStartScript = sh(
                        script: 'node -e "const p = require(\'./package.json\'); if (!p.scripts || !p.scripts.start) process.exit(1);"',
                        returnStatus: true
                    )
                    if (hasStartScript != 0) {
                        error "ERROR: Missing 'start' script in package.json. Please define it to run the server."
                    }
                }
            }
        }

        stage('Start Server') {
            steps {
                echo "Starting the Node.js server..."
                // Jeśli chcesz, żeby serwer działał w tle (np. na stałe po uruchomieniu):
                sh 'nohup npm start > output.log 2>&1 &'
            }
        }
    }

    post {
        success {
            echo '✅ Server started successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above.'
        }
    }
}
