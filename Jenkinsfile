pipeline {
    agent any

    environment {
        GIT_HOME = tool name: 'Git', type: 'GitTool'  // Use the correct Git installation on Jenkins
    }

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                // Checkout the code from GitHub repository
                checkout scm
            }
        }

        stage('Tool Install') {
            steps {
                // Install necessary tools, like Node.js or other dependencies if needed
                script {
                    // Ensure Node.js is installed, or adjust for the tools you require
                    sh 'node -v'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install project dependencies (e.g., npm install for a Node.js project)
                script {
                    // Ensure npm is installed or use the package manager relevant to your project
                    sh 'npm install'
                }
            }
        }

        stage('Lint') {
            steps {
                // Lint the code if you have linting setup
                script {
                    sh 'npm run lint'
                }
            }
        }

        stage('Build') {
            steps {
                // Build the application (e.g., build command for a React or Angular app)
                script {
                    sh 'npm run build'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis for code quality (if you have SonarQube setup)
                script {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Post Actions') {
            steps {
                echo 'Pipeline finished'
            }
        }
    }

    post {
        always {
            cleanWs()  // Cleanup workspace after pipeline completion
        }
        success {
            echo 'Pipeline was successful!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
