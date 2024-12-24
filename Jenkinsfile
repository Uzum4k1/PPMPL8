pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Uzum4k1/PPMPL8.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Unit Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Run Integration Tests') {
            when {
                expression { env.BRANCH_NAME == 'main' } // Integration tests only on 'main'
            }
            steps {
                echo 'Running integration tests...'
                bat 'npm run integration-test'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                // Tambahkan perintah build jika diperlukan
            }
        }

        stage('Deploy') {
            when {
                expression { env.BRANCH_NAME == 'main' } // Deployment only on 'main'
            }
            steps {
                echo 'Deploying the application to staging server...'
                sh '''
                ssh user@staging-server "cd /path/to/app && git pull && npm install && pm2 restart app"
                '''
            }
        }
    }

    post {
        success {
            emailext subject: 'Build Succeeded', body: 'The build succeeded!',
            recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
        failure {
            emailext subject: 'Build Failed', body: 'The build failed.',
            recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
    }
}
