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

        stage('Build with Maven') {
            steps {
                bat '"C:\\apache-maven-3.9.11\\bin\\mvn" clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Windows command to get WAR file
                    def warFile = bat(script: 'dir /b target\\*.war', returnStdout: true).trim()
                    bat """
                        curl -u %TOMCAT_USER%:%TOMCAT_PASS% ^
                        --upload-file target\\${warFile} ^
                        "%TOMCAT_URL%/deploy?path=/pipelineapp&update=true"
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