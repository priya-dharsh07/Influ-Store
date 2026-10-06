pipeline {
    agent any

    tools {
        nodejs 'NodeJS-24'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Generate Prisma Client') {
            steps {
                bat 'npx prisma generate'
            }
        }
        stage('Test / Validation') {
            steps {
                bat 'npm run lint'
            }
        }
        stage('Build Application') {
            steps {
                bat 'npm run build'
            }
        }
        stage('Docker Build') {
            steps {
                bat 'docker build -t influstore:%BUILD_NUMBER% .'
            }
        }
    }
    post {
        success {
            echo 'CI pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Deployment stages will not run.'
        }
    }
}
