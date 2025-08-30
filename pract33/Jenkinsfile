// Jenkinsfile - Declarative pipeline (Node.js example)
pipeline {
  agent any

  environment {
    // set env vars if needed
    APP_ENV = 'development'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        script {
          // if agent is Windows, replace with bat
          if (isUnix()) {
            sh 'npm ci' // or 'npm install' if you prefer
          } else {
            bat 'npm ci'
          }
        }
      }
    }

    stage('Build') {
      steps {
        script {
          if (isUnix()) {
            sh 'npm run build || echo "no build script, continuing"'
          } else {
            bat 'npm run build || echo "no build script, continuing"'
          }
        }
      }
    }

    stage('Test') {
      steps {
        script {
          if (isUnix()) {
            sh 'npm test || true' // do not fail pipeline on test failures if you want to continue; remove "|| true" to fail
          } else {
            bat 'npm test || echo "tests returned non-zero"'
          }
        }
      }
      post {
        always {
          junit '**/test-results/*.xml'  // if your tests produce JUnit xml
          archiveArtifacts artifacts: 'coverage/**,dist/**', allowEmptyArchive: true
        }
      }
    }

    stage('Deploy (staging/simulated)') {
      steps {
        script {
          if (isUnix()) {
            // demo deploy - adjust to your real deploy step (scp, docker push, kubectl, etc.)
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
          echo "Deployment stage finished successfully."
        }
      }
    }
  } // stages

  post {
    always {
      echo "Pipeline finished. Cleaning workspace."
      cleanWs()
    }
    success {
      echo 'Pipeline succeeded.'
    }
    failure {
      echo 'Pipeline failed.'
    }
  }
}
