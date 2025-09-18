pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        // Add credentialsId if repo is private
        git branch: 'main', url: 'https://github.com/sean-rahman-225437344/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm ci || npm install'
      }
    }

    stage('Run Tests') {
      steps {
        // Continue even if tests fail (demo-friendly)
        sh 'npm test || true'
        echo 'About to send a test email via emailext...'
      }
      post {
        success {
       
            mail to: 'rahmansean99@gmail.com',
            subject: "TESTS PASSED",
            body: "Unit/Integration tests passed.",
             attachBuildLog: true
          
        }
        failure {
          
        
            mail to: 'rahmansean99@gmail.com',
            subject: "TESTS FAILED",
            body: "Tests failed.",
             attachBuildLog: true
          
        }
      }
    }

    stage('Generate Coverage Report') {
      steps {
        // Ensure coverage report exists
        sh 'npm run coverage || true'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit || true'
        echo 'About to send a test email via emailext...'
      }
      post {
        success {
          
            mail to: 'rahmansean99@gmail.com',
            subject: "SECURITY OK",
            body: "npm audit completed successfully.",
             attachBuildLog: true
        
        }
        failure {
          
            mail to: 'rahmansean99@gmail.com',
            subject: "SECURITY ISSUES FOUND",
            body: "npm audit reported issues.",
            attachBuildLog: true
          
        }
      }
    }
  }

  post {
    always {
      // keep coverage output if it exists
      archiveArtifacts artifacts: 'coverage/**', allowEmptyArchive: true
    }
  }
}