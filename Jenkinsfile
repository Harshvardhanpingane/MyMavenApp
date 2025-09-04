pipeline {
    agent any

    tools {
        maven 'Maven 3.9.11'
        jdk 'JDK-17'
    }

    environment {
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'admin123'
        TOMCAT_URL  = 'http://localhost:8080/manager/text'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Harshvardhanpingane/MyMavenApp.git'
            }
        }

        stage('Verify Tools') {
            steps {
                script {
                    echo "Maven Path: ${tool 'Maven 3.9.11'}"
                    echo "JAVA_HOME: ${tool 'JDK-17'}"
                    bat 'java -version'
                    bat 'mvn -version'
                }
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    def mvnHome = tool 'Maven 3.9.11'
                    bat "\"${mvnHome}\\bin\\mvn\" clean package"
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    def mvnHome = tool 'Maven 3.9.11'
                    bat "\"${mvnHome}\\bin\\mvn\" test"
                }
            }
            post {
                always {
                    // Fixed JUnit report path
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    bat """
                        curl -v -u ${TOMCAT_USER}:${TOMCAT_PASS} ^
                        --upload-file target\\MyMavenApp.war ^
                        "${TOMCAT_URL}/deploy?path=/pipelineapp&update=true"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build and Deploy Successful!"
        }
        failure {
            echo "❌ Build or Deploy Failed!"
        }
    }
}
