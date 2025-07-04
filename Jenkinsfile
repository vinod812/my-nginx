pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "vinod812/train-schedule"
    }

    stages {
        
        stage("Checkout from GitHub Repo") {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/vinod812/my-nginx.git']]
                ])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        
        /* If you had a Gradle build, it would go here. But you're using Docker, so no need for Gradle now */
    }

    post {
        success {
            echo 'Build completed successfully.'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
