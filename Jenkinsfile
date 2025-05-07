pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "abhinavprakash783/endtermfrontend"
        DOCKER_HUB_CREDENTIALS = "docker-hub-creds-v7"
        CONTAINER_NAME = "endtermfrontend"
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
                    bat "docker build -t %DOCKER_IMAGE% ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}",
                                                      usernameVariable: 'DOCKER_USERNAME',
                                                      passwordVariable: 'DOCKER_PASSWORD')]) {
                        bat "docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    bat "docker push %DOCKER_IMAGE%"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    bat "docker stop %CONTAINER_NAME% || exit 0"
                    bat "docker rm %CONTAINER_NAME% || exit 0"
                    bat "docker run -d -p %HOST_PORT%:80 --name %CONTAINER_NAME% %DOCKER_IMAGE%"
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
