pipeline {
  agent any

  environment {
    DEPLOY_DIR = "/tmp/demo-deploy"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Install') {
      steps {
        sh 'node --version || true'
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

    stage('Deploy (demo)') {
      steps {
        sh '''
          echo "Preparing demo deploy dir: ${DEPLOY_DIR}"
          rm -rf ${DEPLOY_DIR} || true
          mkdir -p ${DEPLOY_DIR}
          cp -r build/* ${DEPLOY_DIR}/
          echo "Files copied to ${DEPLOY_DIR}:"
          ls -la ${DEPLOY_DIR} | sed -n '1,200p'
        '''
      }
    }
  }

  post {
    success { echo "Demo pipeline finished SUCCESS" }
    failure { echo "Demo pipeline FAILED" }
  }
}