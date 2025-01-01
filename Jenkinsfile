pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Jenkins Node.js installation
    }

    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs\\'  // Path to Node.js installation
        SONAR_SCANNER_PATH = 'C:\\Users\\akash\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
        PATH = "${env.NODEJS_HOME};${env.SONAR_SCANNER_PATH};${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Quality Checks') {
            parallel {
                stage('Lint') {
                    steps {
                        bat 'npm run lint'
                    }
                }

                stage('Build') {
                    steps {
                        bat 'npm run build'
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Jenkins credential for SonarQube token
            }
            steps {
                bat '''
                where sonar-scanner || (echo "SonarQube scanner not found" && exit /b 1)
                sonar-scanner -Dsonar.projectKey=mernfrontend ^
                              -Dsonar.sources=. ^
                              -Dsonar.host.url=http://localhost:9000 ^
                              -Dsonar.login=%SONAR_TOKEN%
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully. Build and quality checks passed.'
        }
        failure {
            echo 'Pipeline failed. Review logs to identify the issue.'
        }
        always {
            archiveArtifacts artifacts: '**/build/**/*', allowEmptyArchive: true
            echo 'Build artifacts archived.'
        }
    }
}
