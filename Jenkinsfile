pipeline {
  agent any

  environment {
    NODE_OPTIONS = '--max_old_space_size=4096'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Lint') {
      steps {
        sh 'npm run lint'
      }
    }

    stage('Docker smoke test') {
      steps {
        sh 'docker compose -f docker-compose.test.yml up --build --exit-code-from sut'
      }
      post {
        always {
          sh 'docker compose -f docker-compose.test.yml down --remove-orphans'
        }
      }
    }
  }
}
