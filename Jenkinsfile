pipeline {
  agent any

  tools {nodejs "node"}

  environment {
    POSTMAN_API_KEY = credentials('API_KEY_ONE')  // Store API Key securely in Jenkins credentials
  }

  stages {
    stage('Install Postman CLI') {
      steps {
        sh 'curl -o- "https://dl-cli.pstmn.io/install/linux64.sh" | sh'
      }
    }

    stage('Login to Postman CLI') {
      steps {
        sh 'postman login --with-api-key $POSTMAN_API_KEY'
      }
    }

    stage('Run API Tests') {
      steps {
        sh 'postman collection run "Khusm API Testing.postman_collection.json" -e "Khusm API Environment.postman_environment.json"'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/newman/*.json', fingerprint: true
    }
  }
}
