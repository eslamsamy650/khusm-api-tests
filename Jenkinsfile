pipeline {
  agent any

  tools {nodejs "node"}

  environment {
    POSTMAN_API_KEY = credentials('POSTMAN_API_KEY')  // Securely store API Key in Jenkins credentials
  }

  stages {
    stage('Install Postman CLI') {
      steps {
        bat '''
          curl -o postman-cli.zip "https://dl-cli.pstmn.io/install/win64.zip"
          powershell -Command "Expand-Archive -Path postman-cli.zip -DestinationPath C:\\PostmanCLI -Force"
          echo "Postman CLI Installed"
        '''
      }
    }

    stage('Login to Postman CLI') {
      steps {
        bat 'C:\\PostmanCLI\\postman login --with-api-key %POSTMAN_API_KEY%'
      }
    }

    stage('Run API Tests') {
      steps {
        bat 'C:\\PostmanCLI\\postman collection run "Khusm API Testing.postman_collection.json" -e "Khusm API Environment.postman_environment.json"'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/newman/*.json', fingerprint: true
    }
  }
}
