pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "spring-ci:1.0"
        CONTAINER_NAME = "spring-ci-container"
        HOST_PORT = "8080"
        CONTAINER_PORT = "8080"
    }

    stages {
        stage('Hello World') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:azizmabrouki/atelier-jenkins.git',
                    credentialsId: 'jenkins-ssh-key'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Run') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} ."

                    // Stop and remove container if it already exists
                    sh """
                    if [ \$(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                        docker stop ${CONTAINER_NAME}
                        docker rm ${CONTAINER_NAME}
                    fi
                    if [ \$(docker ps -aq -f name=${CONTAINER_NAME}) ]; then
                        docker rm ${CONTAINER_NAME}
                    fi
                    """

                    // Run container
                    sh "docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
