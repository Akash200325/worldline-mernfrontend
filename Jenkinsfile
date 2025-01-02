pipeline {
    agent any

    tools {
        nodejs 'Nodejs' // Assuming nodejs is installed and configured in Jenkins tool config
    }

    environment {
        GIT_REPO_URL = 'https://github.com/Akash200325/worldline-mernfrontend.git'
        BRANCH = 'main'
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = 'sqp_70213d5076238bc4f6ede8212afa725da7fcd5d2'
        SONAR_PROJECT_KEY = 'mernstackfrontend'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm ci'  // Use npm ci to install dependencies from package-lock.json
            }
        }

        stage('Lint') {
            steps {
                bat 'npm run lint'  // Run lint using npm in Windows
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'  // Build the project using npm in Windows
            }
        }

        stage('SonarQube Analysis') {
            steps {
                bat """
                    sonar-scanner.bat -Dsonar.projectKey=${SONAR_PROJECT_KEY} -Dsonar.sources=. -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.token=${SONAR_TOKEN}
                """  // Run SonarQube analysis using the provided command
            }
        }
    }

    post {
        always {
            echo 'This will run regardless of the pipeline result.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
