pipeline {
    agent any

    parameters {
        string(name: 'DOCKERHUB_USERNAME', defaultValue: 'your-dockerhub-username', description: 'Docker Hub username used for the image repository.')
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
                sh '''
                    docker build \
                        -t ${DOCKERHUB_USERNAME}/${IMAGE_REPOSITORY}:${BUILD_NUMBER} \
                        -t ${DOCKERHUB_USERNAME}/${IMAGE_REPOSITORY}:latest \
                        .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKERHUB_USERNAME}/${IMAGE_REPOSITORY}:${BUILD_NUMBER}
                        docker push ${DOCKERHUB_USERNAME}/${IMAGE_REPOSITORY}:latest
                    '''
                }
            }
        }

        stage('Run Three Containers') {
            steps {
                sh '''
                    for port in 8082 8083 8084; do
                        docker rm -f cs304-practice10-${port} 2>/dev/null || true
                        docker run -d --name cs304-practice10-${port} -p ${port}:80 ${DOCKERHUB_USERNAME}/${IMAGE_REPOSITORY}:latest
                    done
                    docker ps --filter "name=cs304-practice10"
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
