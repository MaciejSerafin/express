pipeline {
    agent any

    environment {
        // Definicje zmiennych środowiskowych, jeśli potrzeba
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out the repository..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                // Jeśli aplikacja wymaga skryptu build, a nie ma go, można to pominąć lub dodać odpowiednią komendę
                sh 'npm run build || echo "No build script found"'
            }
        }

        stage('Archive Build') {
            steps {
                echo "Archiving build artifact..."
                // Tworzenie nowego katalogu do archiwizacji
                sh 'mkdir -p /root/express-artifacts'
                sh 'tar -czf /root/express-artifacts/express.tar.gz .'
                echo "Artifact archived as /root/express-artifacts/express.tar.gz"
                archiveArtifacts artifacts: '/root/express-artifacts/express.tar.gz', allowEmptyArchive: true
            }
        }

        stage('Start Server') {
            steps {
                echo "Starting the server..."
                // Możesz dodać komendę uruchomienia serwera, np.:
                // sh 'npm start'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Uruchamianie testów, np.:
                // sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                // Proces wdrożenia aplikacji, np.:
                // sh 'some-deploy-command'
            }
        }
    }

    post {
        always {
            echo "Pipeline finished!"
        }
        success {
            echo "Pipeline succeeded!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
