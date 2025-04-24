pipeline {
    agent any

    environment {
        NODE_ENV = 'test'
    }

    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/MaciejSerafin/express.git', branch: 'master'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run tests') {
            steps {
                sh 'npm test'
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: '**/test-results.xml'
        }
    }
}
