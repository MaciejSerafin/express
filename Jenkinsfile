pipeline {
    agent any

    environment {
        APP_NAME = 'express-app'
        DEPLOY_DIR = "/tmp/${APP_NAME}"
        ARTIFACT = "express.tar.gz"
    }

    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/MaciejSerafin/express.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    mkdir -p $DEPLOY_DIR
                    cp -r * $DEPLOY_DIR
                '''
            }
        }

        stage('Publish') {
            steps {
                sh '''
                    tar -czf $ARTIFACT $DEPLOY_DIR
                    echo "Artifact published to workspace: $ARTIFACT"
                '''
                archiveArtifacts artifacts: "${ARTIFACT}", fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
