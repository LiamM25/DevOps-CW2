pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'liamm25/devops-cw2'
        DOCKERHUB_CREDENTIALS = 'liamm25' 
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using credentials
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        docker.build(env.DOCKER_IMAGE_NAME, '-f Dockerfile .')
                    }
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                script {
                    // Test the Docker image by running a simple command (e.g., echo)
                    docker.image(env.DOCKER_IMAGE_NAME).inside {
                        sh 'echo "Container launched successfully"'
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    // Push the Docker image to DockerHub using credentials
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        sh "docker push ${env.DOCKER_IMAGE_NAME}"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Deploy to Kubernetes using kubectl
                    sh "kubectl create deployment devops-cw2 --image=${env.DOCKER_IMAGE_NAME}:latest"
                    sh "kubectl expose deployment devops-cw2 --type=NodePort --port=8080"
                }
            }
        }
    }

    post {
        success {
            echo 'Build, test, push, and deploy successful!'
        }
    }
}
