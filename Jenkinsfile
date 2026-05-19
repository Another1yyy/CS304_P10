pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-credentials'
        IMAGE_REPOSITORY = 'cs304-practice10'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                    docker build ^
                        -t %DOCKER_USER%/%IMAGE_REPOSITORY%:%BUILD_NUMBER% ^
                        -t %DOCKER_USER%/%IMAGE_REPOSITORY%:latest ^
                        .
                    '''
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        docker push %DOCKER_USER%/%IMAGE_REPOSITORY%:%BUILD_NUMBER%
                        if errorlevel 1 exit /b 1
                        docker push %DOCKER_USER%/%IMAGE_REPOSITORY%:latest
                        if errorlevel 1 exit /b 1
                    '''
                }
            }
        }

        stage('Run Three Containers') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                    for %%p in (8082 8083 8084) do (
                        docker rm -f cs304-practice10-%%p 2>NUL
                        docker run -d --name cs304-practice10-%%p -p %%p:80 %DOCKER_USER%/%IMAGE_REPOSITORY%:latest
                    )
                    docker ps --filter "name=cs304-practice10"
                    '''
                }
            }
        }
    }

    post {
        always {
            bat 'docker ps --filter "name=cs304-practice10" || exit /b 0'
        }
    }
}
