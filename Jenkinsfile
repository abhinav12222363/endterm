pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "abhinavprakash783/premiumbagfrontend"
        DOCKER_HUB_CREDENTIALS = "docker-hub-creds-v7"
        CONTAINER_NAME = "premiumbagfrontend"
        HOST_PORT = "8082"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/abhinav12222363/endterm'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Use 'bat' for Windows and 'sh' for non-Windows agents
                    if (isUnix()) {
                        sh "docker build -t ${DOCKER_IMAGE} ."
                    } else {
                        bat "docker build -t %DOCKER_IMAGE% ."
                    }
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: env.DOCKER_HUB_CREDENTIALS,
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD')]) {
                        
                        if (isUnix()) {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        } else {
                            bat "docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%"
                        }
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    if (isUnix()) {
                        sh "docker push ${DOCKER_IMAGE}"
                    } else {
                        bat "docker push %DOCKER_IMAGE%"
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove any existing containers
                    if (isUnix()) {
                        sh "docker stop ${CONTAINER_NAME} || true"
                        sh "docker rm ${CONTAINER_NAME} || true"
                        sh "docker run -d -p ${HOST_PORT}:80 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}"
                    } else {
                        bat "docker stop %CONTAINER_NAME% || exit 0"
                        bat "docker rm %CONTAINER_NAME% || exit 0"
                        bat "docker run -d -p %HOST_PORT%:80 --name %CONTAINER_NAME% %DOCKER_IMAGE%"
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
