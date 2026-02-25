pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select deployment environment'
        )
    }

    environment {
        APP_NAME = "python-app"
        DEPLOY_PATH = "/opt/apps/${params.ENVIRONMENT}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-username/python-app.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building for ${params.ENVIRONMENT}"
                sh 'echo Build completed'
            }
        }

        stage('Manual Approval for PROD') {
            when {
                expression { params.ENVIRONMENT == 'prod' }
            }
            steps {
                input message: "Approve Production Deployment?",
                      ok: "Deploy"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'dev') {
                        echo "Deploying to DEV"
                    }
                    else if (params.ENVIRONMENT == 'qa') {
                        echo "Deploying to QA"
                    }
                    else {
                        echo "Deploying to PROD"
                    }
                }

                withCredentials([usernamePassword(
                    credentialsId: 'server-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh '''
                        echo "Connecting using secured credentials"
                        # Example deployment command
                        # ssh $USER@server "deploy command"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful 🚀"
        }
        failure {
            echo "Deployment Failed ❌"
        }
        always {
            cleanWs()
        }
    }
}