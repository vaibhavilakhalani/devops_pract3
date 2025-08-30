// Jenkinsfile - Declarative pipeline (Node.js example)

pipeline {
    agent any

    environment {
        APP_ENV = 'development'  // Example environment variable
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Debug Workspace') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'ls -la'
                    } else {
                        bat 'dir'
                    }
                }
            }
        }

        stage('Install') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm install'
                    } else {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm run build || echo "No build script, continuing..."'
                    } else {
                        bat 'npm run build || echo "No build script, continuing..."'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        // Allow pipeline to continue even if tests fail (remove "|| true" to fail on tests)
                        sh 'npm test || true'
                    } else {
                        bat 'npm test || echo "Tests returned non-zero"'
                    }
                }
            }
            post {
                always {
                    // Collect test results if available
                    junit '**/test-results/*.xml'
                    archiveArtifacts artifacts: 'coverage/**, dist/**', allowEmptyArchive: true
                }
            }
        }

        stage('Deploy (Simulated)') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            echo "Deploying application (simulated)..."
                            mkdir -p deploy_output
                            echo "Deployed at $(date)" > deploy_output/deploy-info.txt
                            ls -la
                        '''
                    } else {
                        bat '''
                            echo Deploying application (simulated)...
                            if not exist deploy_output mkdir deploy_output
                            echo Deployed at %DATE% %TIME% > deploy_output\\deploy-info.txt
                            dir
                        '''
                    }
                }
            }
            post {
                success {
                    echo "✅ Deployment stage finished successfully."
                }
            }
        }
    }

    post {
        always {
            echo "🧹 Cleaning workspace..."
            cleanWs()
        }
        success {
            echo "🎉 Pipeline succeeded!"
        }
        failure {
            echo "❌ Pipeline failed."
        }
    }
}
