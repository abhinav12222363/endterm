pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nightsight30/premiumbagfrontend"
        DOCKER_HUB_CREDENTIALS = "docker-hub-creds-v3"
        CONTAINER_NAME = "premiumbagfrontend"
        HOST_PORT = "8082"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/nightsight30-sky/premiumbagFrontend'
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
