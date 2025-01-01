pipeline {
    agent any

    tools {
        nodejs 'nodejs'
    }

    environment {
        NODEJS_HOME = 'C:\\\\Program Files\\\\nodejs\\\\'
        SONAR_SCANNER_PATH = 'C:\\\\Users\\\\akash\\\\Downloads\\\\sonar-scanner-cli-6.2.1.4610-windows-x64\\\\sonar-scanner-6.2.1.4610-windows-x64\\\\bin'
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
                bat 'npm install'
            }
        }

        stage('Lint') {
            steps {
                bat 'npm run lint'
            }
        }

        stage('Build') {
            steps {
                script {
                    try {
                        bat 'npm run build'
                    } catch (Exception e) {
                        error "Build failed: ${e.message}"
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')
            }
            steps {
                bat """
                sonar-scanner -Dsonar.projectKey=mernfrontend ^
                              -Dsonar.sources=. ^
                              -Dsonar.host.url=http://localhost:9000 ^
                              -Dsonar.login=${env.SONAR_TOKEN}
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
            script {
                if (fileExists('build')) {
                    archiveArtifacts artifacts: '**/build/**/*', allowEmptyArchive: true
                } else {
                    echo 'Build directory does not exist. Skipping artifact archiving.'
                }
            }
        }
    }
}
