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

        stage('Build') {
            steps {
                echo "Installing dependencies and building..."
                script {
                    // Sprawdzenie czy plik package.json istnieje
                    if (!fileExists('package.json')) {
                        echo "Brak pliku package.json w katalogu repozytorium!"
                        error "Brak pliku package.json, niemożliwe jest zbudowanie aplikacji!"
                    }
                    
                    // Instalacja zależności i budowanie
                    sh 'npm install'
                    sh 'npm run build' // Jeśli masz skrypt build w package.json, zmień wg potrzeb
                }
            }
        }

        stage('Archive Build') {
            steps {
                echo "Archiving build artifact..."
                // Spakowanie aplikacji
                sh 'tar -czf express.tar.gz -C $APP_DIR express-app'
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
                    sh 'cd /root/express-run && npm start' // Uruchomienie aplikacji w przypadku Node.js
                }
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Uruchomienie testów jeśli są zdefiniowane
                sh 'npm test' // Zmieniaj w zależności od konfiguracji
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the application..."
                // Twoje kroki wdrożeniowe
                // Na przykład, może to być upload na serwer produkcyjny
                // sh 'scp express-app.tar.gz user@server:/path/to/deploy'
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
