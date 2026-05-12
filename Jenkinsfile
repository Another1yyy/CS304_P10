pipeline {
    agent any

    parameters {
        string(name: 'DOCKERHUB_USERNAME', defaultValue: 'Another1yyy', description: 'Docker Hub username used for the image repository.')
    }

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
                bat '''
                    docker build ^
                        -t %DOCKERHUB_USERNAME%/%IMAGE_REPOSITORY%:%BUILD_NUMBER% ^
                        -t %DOCKERHUB_USERNAME%/%IMAGE_REPOSITORY%:latest ^
                        .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker push %DOCKERHUB_USERNAME%/%IMAGE_REPOSITORY%:%BUILD_NUMBER%
                        docker push %DOCKERHUB_USERNAME%/%IMAGE_REPOSITORY%:latest
                    '''
                }
            }
        }

        stage('Run Three Containers') {
            steps {
                bat '''
                    for %%p in (8082 8083 8084) do (
                        docker rm -f cs304-practice10-%%p 2>NUL
                        docker run -d --name cs304-practice10-%%p -p %%p:80 %DOCKERHUB_USERNAME%/%IMAGE_REPOSITORY%:latest
                    )
                    docker ps --filter "name=cs304-practice10"
                '''
            }
        }
    }

    post {
        always {
            bat 'docker logout || exit /b 0'
        }
    }
}
