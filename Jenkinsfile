pipeline {
  agent any

  tools { nodejs "node" }

  environment {
    POSTMAN_API_KEY = credentials('POSTMAN_API_KEY')  // Securely store API Key in Jenkins credentials
  }

  stages {
    stage('Download & Install Postman CLI') {
      steps {
        bat '''
          curl -o postman-cli.exe "https://dl-cli.pstmn.io/install/win64"
          mkdir C:\\PostmanCLI 2>nul
          move /Y postman-cli.exe C:\\PostmanCLI\\postman.exe
        '''
      }
    }

    stage('Verify Installation') {
      steps {
        bat '''
          if exist C:\\PostmanCLI\\postman.exe (
            echo "Postman CLI installed successfully"
          ) else (
            echo "Postman CLI installation failed" && exit 1
          )
        '''
      }
    }

    stage('Login to Postman CLI') {
  steps {
    powershell 'Start-Process -NoNewWindow -FilePath "C:\\PostmanCLI\\postman.exe" -ArgumentList "login --with-api-key PMAK-xxxxx" -Wait'
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
