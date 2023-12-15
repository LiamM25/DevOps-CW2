pipeline {
    agent any

    environment {
	DOCKER_IMAGE_NAME = 'liamm25/devops-cw2:latest'
        DOCKER_COMMON_CREDS = credentials('liam-dockerhub') 
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
		  
                sh 'docker login -u $DOCKER_COMMON_CREDS_USR -p $DOCKER_COMMON_CREDS_PSW'
                sh 'docker push liamm25/devops-cw2:1.0'
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
