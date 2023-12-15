pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'liamm25/devops-cw2'
        DOCKERHUB_CREDENTIALS = credentials('liam-dockerhub')  // Add your DockerHub credentials ID
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE_NAME, '-f Dockerfile .')
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                script {
                    docker.image(env.DOCKER_IMAGE_NAME).inside {
                        sh 'echo "Container launched successfully"'
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    // Authenticate Docker to DockerHub using credentials
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        // Get the Docker image reference
                        def dockerImage = docker.image(env.DOCKER_IMAGE_NAME)

                        // Push the Docker image to DockerHub
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl create deployment devops-cw2 --image=liamm25/devops-cw2:latest"
            }
        }
    }

    post {
        success {
            echo 'Build, test, push, and optionally deploy successful!'
        }
    }
}
