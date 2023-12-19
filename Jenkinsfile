pipeline{
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'liamm25/devops-cw2:latest'
        DOCKER_COMMON_CREDS = credentials('liam-dockerhub')
    }

    stages {
        stage('Build & test Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE_NAME, '-f Dockerfile Build Successful!')
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                // Use DOCKER_COMMON_CREDS_USR and DOCKER_COMMON_CREDS_PSW
                sh "docker login -u ${DOCKER_COMMON_CREDS_USR} -p ${DOCKER_COMMON_CREDS_PSW}"
                sh "docker push ${env.DOCKER_IMAGE_NAME}"
            }
        }


	stage('Deploy to Kubernetes') {
            steps {
                sshagent(['Production_key']) {

                    sh '''
                    ssh ubuntu@3.89.251.183 '/usr/bin/kubectl set image deployments/devops-cw2 devops-cw2=liamm25/devops-cw2:latest'
                    '''

                }
            }
        }
    }
}
