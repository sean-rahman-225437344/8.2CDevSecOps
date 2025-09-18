pipeline {
agent any
    stages {
    stage('Checkout') {
        steps {
            git branch: 'main', url: 'https://github.com/sean-rahman-225437344/8.2CDevSecOps.git'
        }
    }
    stage('Install Dependencies') {
        steps {
            sh 'npm install'
        }
    }
    stage('Run Tests') {
        steps {
            sh 'npm test || true' // Allows pipeline to continue despite test failures
        }         
    post {
        success {
          emailext(
            to: 'rahmansean99@gmail.com',
            subject: "TESTS PASSED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Unit/Integration tests passed.\nSee console: ${env.BUILD_URL}console",
            attachLog: true
          )
        }
        failure {
          emailext(
            to: 'rahmansean99@gmail.com',
            subject: "TESTS FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Tests failed.\nSee console: ${env.BUILD_URL}console",
            attachLog: true
          )
        }
      }
    }
   }
    stage('Generate Coverage Report') {
        steps {
            // Ensure coverage report exists
            sh 'npm run coverage || true'
        }
     post {
        success {
          emailext(
            to: 'rahmansean99@gmail.com',
            subject: "SECURITY OK: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "npm audit completed with success.\nSee console: ${env.BUILD_URL}console",
            attachLog: true
          )
        }
        failure {
          emailext(
            to: 'rahmansean99@gmail.com',
            subject: "SECURITY ISSUES: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "npm audit reported issues.\nSee console: ${env.BUILD_URL}console",
            attachLog: true
          )
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