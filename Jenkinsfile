pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Ensure 'nodejs' is correctly configured in Jenkins tools
    }

    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs\\'
        SONAR_SCANNER_PATH = 'C:\\Users\\akash\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
        PATH = "${env.NODEJS_HOME};${env.SONAR_SCANNER_PATH};${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    try {
                        checkout scm
                    } catch (Exception e) {
                        error "Checkout failed: ${e.message}"
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                npm install
                '''
            }
        }

        stage('Lint') {
            steps {
                bat '''
                npm run lint
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                npm run build
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                bat """
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner -Dsonar.projectKey=mernfrontend -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.token=${SONAR_TOKEN}
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for errors.'
        }
        always {
            archiveArtifacts artifacts: '**/build/**/*', allowEmptyArchive: true
            echo 'Archived build artifacts.'
        }
    }
}
