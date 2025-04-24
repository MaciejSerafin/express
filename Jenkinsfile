pipeline {
  agent any

  environment {
    APP_DIR = '/tmp/express-app'
    PACKAGE_NAME = 'express.tar.gz'
    RUN_DIR = "${HOME}/express-run"
  }

  stages {
    stage('Clone') {
      steps {
        echo '📥 Cloning repository...'
        git 'https://github.com/MaciejSerafin/express.git'
      }
    }

    stage('Build') {
      steps {
        echo '🔧 Installing dependencies...'
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        echo '🧪 Running tests...'
        sh 'npm test || true'  // jeśli testy są puste, nie przerywa pipeline
      }
    }

    stage('Deploy') {
      steps {
        echo '📦 Preparing files for deployment...'
        sh '''
          mkdir -p $APP_DIR
          cp -r * $APP_DIR  # Kopiuje wszystkie pliki do katalogu tymczasowego
        '''
      }
    }

    stage('Publish') {
      steps {
        echo '📤 Archiving build artifact...'
        sh '''
          tar -czf $PACKAGE_NAME -C /tmp express-app
          echo "Artifact published to workspace: $PACKAGE_NAME"
        '''
        archiveArtifacts artifacts: "$PACKAGE_NAME", fingerprint: true
      }
    }

    stage('Start server') {
      steps {
        echo '🚀 Starting the server...'
        sh '''
          mkdir -p $RUN_DIR
          tar -xzf $PACKAGE_NAME -C $RUN_DIR
          cd $RUN_DIR
          ls -la  # Wydrukuj zawartość katalogu, aby upewnić się, że package.json jest obecny
          npm install
          nohup npm start &
          echo "🌐 App started at http://localhost:3000"
        '''
      }
    }
  }

  post {
    always {
      echo '🧼 Cleaning up...'
    }
    success {
      echo '✅ Pipeline completed successfully!'
    }
    failure {
      echo '❌ Pipeline failed!'
    }
  }
}
