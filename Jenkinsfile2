pipeline {
    agent any

    environment {
        APP_NAME = "python-app"
        BUILD_VERSION = "${BUILD_NUMBER}"
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/upendralearn2026-cell/jenkins-demo1.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r python-app/requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest --junitxml=report.xml
                '''
            }
            post {
                always {
                    junit 'report.xml'
                }
            }
        }

        stage('Build Artifact') {
            steps {
                sh '''
                    zip -r ${APP_NAME}-${BUILD_VERSION}.zip .
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '*.zip', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Build Successful 🎉"
        }
        failure {
            echo "Build Failed ❌"
            mail to: 'upendralearn2025@gmail.com',
                 subject: "Build Failed: ${JOB_NAME} #${BUILD_NUMBER}",
                 body: "Check Jenkins Console Output"
        }
        always {
            cleanWs()
        }
    }
}