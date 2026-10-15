pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "keerthanamr08/keeru"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                         url: 'https://github.com/keerthanamr08/exp1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:v1 .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(
                    credentialsId: 'DockerCredential',
                    variable: 'DOCKER_PAT'
                )]) {
                    bat '''
                    @echo off
                    echo %DOCKER_PAT%|docker login -u keerthanamr08 --password-stdin
                   if errorlevel 1 exit /b 1
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%:v1'
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
