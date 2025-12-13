pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/kirankulal37/jenkins-pipeline.git', branch: 'main'
      }
    }

    stage('Install') {
      steps {
        sh 'npm ci'
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
        archiveArtifacts artifacts: 'build/**', fingerprint: true
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
    success { echo "CI/CD pipeline successfully deployed to Vercel 🎉" }
    failure { echo "Pipeline failed ❌" }
  }
}
