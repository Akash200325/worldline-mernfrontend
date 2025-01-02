pipeline {
    agent any
    tools {
        nodejs 'nodejs' // Name of the Node.js installation in Jenkins
    }

    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs\\'  // Set the Node.js path
        SONAR_SCANNER_PATH = 'C:\\Users\\akash\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }

        stage('Lint') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run lint
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner -Dsonar.projectKey=mernstackfrontend ^
                              -Dsonar.sources=. ^
                              -Dsonar.host.url=http://localhost:9000 ^
                              -Dsonar.token=sqp_70213d5076238bc4f6ede8212afa725da7fcd5d2
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
