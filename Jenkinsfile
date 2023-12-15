pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'liamm25/devops-cw2'
        DOCKERHUB_USERNAME = credentials('liam-dockerhub').username
        DOCKERHUB_PASSWORD = credentials('liam-dockerhub').password
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
                    // Login to DockerHub
                    sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"

                    // Push the Docker image to DockerHub
                    sh "docker push ${env.DOCKER_IMAGE_NAME}:1.0"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl create deployment devops-cw2 --image=${env.DOCKER_IMAGE_NAME}:1.0"
            }
        }
    }

    post {
        success {
            echo 'Build, test, push, and optionally deploy successful!'
        }
    }
}
