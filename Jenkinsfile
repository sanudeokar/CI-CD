pipeline {
    agent any
    environment {
        DOCKER_HOST = 'tcp://localhost:2375'
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
    }
    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/sanudeokar/ci-cd'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.withServer("${DOCKER_HOST}") {
                        docker.build('sanudeokar/ci-cd:latest')
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withServer("${DOCKER_HOST}") {
                        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials-id') {
                            docker.image('sanudeokar/ci-cd:latest').push()
                        }
                    }
                }
            }
        }
        stage('Deploy Docker Container') {
            steps {
                script {
                    docker.withServer("${DOCKER_HOST}") {
                        docker.image('sanudeokar/ci-cd:latest').run('-d -p 8081:8080')
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
