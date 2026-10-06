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
        stage('Test / Build Validation') {
            steps {
                bat 'npm run build'
            }
        }
        stage('Docker Build') {
            steps {
                bat 'docker build -t influstore:%BUILD_NUMBER% .'
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat 'docker login -u "%DOCKER_USERNAME%" -p "%DOCKER_PASSWORD%"'
                    bat 'docker tag influstore:%BUILD_NUMBER% %DOCKER_USERNAME%/influstore:%BUILD_NUMBER%'
                    bat 'docker push %DOCKER_USERNAME%/influstore:%BUILD_NUMBER%'
                    bat 'docker logout'
                }
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