pipeline {
  agent any

  tools { nodejs "node" }

  environment {
    POSTMAN_API_KEY = credentials('POSTMAN_API_KEY')  // Securely store API Key in Jenkins credentials
  }

  stages {
    stage('Install Postman CLI') {
      steps {
        bat '''
          curl -o postman-cli.zip "https://dl-cli.pstmn.io/install/win64.zip"
          if not exist C:\\PostmanCLI mkdir C:\\PostmanCLI
          powershell -command "Expand-Archive -Path postman-cli.zip -DestinationPath C:\\PostmanCLI -Force" || echo "Extraction failed" && exit 1
          dir C:\\PostmanCLI
        '''
      }
    }

    stage('Verify Installation') {
      steps {
        bat '''
          if exist C:\\PostmanCLI\\postman-cli.exe (
            echo "Postman CLI installed successfully"
          ) else (
            echo "Postman CLI installation failed" && exit 1
          )
        '''
      }
    }

    stage('Login to Postman CLI') {
      steps {
        bat 'C:\\PostmanCLI\\postman-cli login --with-api-key %POSTMAN_API_KEY%'
      }
    }

    stage('Run API Tests') {
      steps {
        bat 'C:\\PostmanCLI\\postman-cli collection run "Khusm API Testing.postman_collection.json" -e "Khusm API Environment.postman_environment.json"'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/newman/*.json', fingerprint: true
    }
  }
}
