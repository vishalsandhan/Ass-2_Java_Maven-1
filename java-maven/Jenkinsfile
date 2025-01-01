pipeline {
    agent any
    tools {
        maven 'Maven' // Ensure this matches the Maven installation name in Jenkins
    }
    environment {
        SONAR_SCANNER_PATH = 'C:\\Users\\91844\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin\\sonar-scanner.bat'
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_PROJECT_KEY = 'Maven-java'
        SONAR_PROJECT_NAME = 'Maven-java'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Siddhant40/java-maven.git' // Replace with your GitHub repository URL
            }
        }
        stage('Build') {
            steps {
                bat 'mvn -X clean install' // Clean and build the project
            }
        }
        stage('Unit Tests') {
            steps {
                bat 'mvn test' // Run unit tests separately
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('SonarQube') // Jenkins credential ID for SonarQube token
            }
            steps {
                bat """
                    mvn sonar:sonar ^
                    -Dsonar.projectKey=${SONAR_PROJECT_KEY} ^
                    -Dsonar.projectName=${SONAR_PROJECT_NAME} ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=${SONAR_HOST_URL} ^
                    -Dsonar.token=%SONAR_TOKEN%
                """
            }
        }
    }
    post {
        success {
            echo 'Build, testing, and analysis completed successfully!'
        }
        failure {
            echo 'Build, testing, or analysis failed!'
        }
        always {
            echo 'Pipeline execution finished.'
        }
    }
}
