pipeline {
    agent any

    tools {
        nodejs "Node24"   // Name must match your NodeJS tool config in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/karthikeyabollineni04/frontend.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Archive build artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }

        // Optional: Deploy
        stage('Deploy to GitHub Pages') {
            when {
                branch 'main'
            }
            steps {
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) {
                    sh 'npm run deploy'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build Successful"
        }
        failure {
            echo "❌ Pipeline Failed"
        }
    }
}
