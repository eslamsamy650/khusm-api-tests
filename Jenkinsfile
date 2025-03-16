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
        bat 'C:\\PostmanCLI\\postman login --with-api-key %POSTMAN_API_KEY%'
      }
    }

    stage('Run API Tests') {
      steps {
    
