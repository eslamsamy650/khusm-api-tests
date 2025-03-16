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
                    if not exist C:\\PostmanCLI mkdir C:\\PostmanCLI
                    curl -o C:\\PostmanCLI\\postman.exe "https://dl-cli.pstmn.io/install/win64"
                    icacls C:\\PostmanCLI\\postman.exe /grant Everyone:F
                '''
            }
        }

        stage('Verify Installation') {
            steps {
                bat '''
                    if exist C:\\PostmanCLI\\postman.exe (
                        echo "Postman CLI installed successfully"
                        C:\\PostmanCLI\\postman --version || echo "Warning: Postman CLI may not be executable!"
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
                bat '''
                    if not exist reports mkdir reports
                    C:\\PostmanCLI\\postman collection run C:\\path\\to\\your_collection.json -e C:\\path\\to\\your_environment.json --reporters json --reporter-json-export reports/postman-report.json
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'reports/*.json', fingerprint: true
        }
    }
}
