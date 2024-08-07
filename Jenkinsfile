pipeline {
    agent any
    environment {
        DOCKER_HOST = 'tcp://localhost:2375'
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
    }
    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/sanudeokar/CI-CD'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.withServer("${DOCKER_HOST}") {
                        docker.build('sanudeokar/CI-CD:latest')
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withServer("${DOCKER_HOST}") {
                        docker.withRegistry('https://registry.hub.docker.com', 'DOCKERHUB_CREDENTIALS') {
                            docker.image('sanudeokar/CI-CD:latest').push()
                        }
                    }
                }
            }
        }
        stage('Deploy Docker Container') {
            steps {
                script {
                    docker.withServer("${DOCKER_HOST}") {
                        docker.image('sanudeokar/CI-CD:latest').run('-d -p 8080:8080')
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
