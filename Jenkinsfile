pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test -- --watchAll=false || true'
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }

    stage('Deploy to Vercel') {
      steps {
        withCredentials([string(credentialsId: 'VERCEL_TOKEN', variable: 'VERCEL_TOKEN')]) {
          sh '''
            echo "Deploying to Vercel..."
            vercel deploy --prod --token=$VERCEL_TOKEN --yes
          '''
        }
      }
    }
  }

  post {
    success { echo "CI/CD dployed succesfully to Vercel " }
    failure { echo "failed " }
  }
}
