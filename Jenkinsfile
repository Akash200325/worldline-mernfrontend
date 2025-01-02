pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Configure your NodeJS installation in Jenkins
    }

    environment {
        SONARQUBE_SERVER = 'sonarqube' // Replace with your SonarQube server configuration name
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Akash200325/worldline-mernfrontend.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    sh 'npm run lint'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
        }

        stage('SonarQube Analysis') {
            when {
                not {
                    anyOf {
                        failed();
                        aborted();
                    }
                }
            }
             steps {
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
               sonar-scanner -Dsonar.projectKey=mernstackfrontend 
                             -Dsonar.sources=.  
                             -Dsonar.host.url=http://localhost:9000  
                             -Dsonar.token=sqp_70213d5076238bc4f6ede8212afa725da7fcd5d2
                '''
            }
        }
    }

    post {
        always {
            echo 'This runs regardless of the pipeline result.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
