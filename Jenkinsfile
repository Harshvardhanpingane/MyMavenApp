pipeline {
    agent any
    
    tools {
        maven 'Maven 3.9.11'
        jdk 'JDK-17'
    }

    environment {
        SONARQUBE = 'SonarQube'   // The name you configured
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Harshvardhanpingane/MyMavenApp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=pipeline-demo'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
    }

    post {
        success {
            echo "✅ Build + Test + SonarQube Analysis Passed!"
        }
        failure {
            echo "❌ Pipeline Failed!"
        }
    }
}