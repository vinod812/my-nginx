pipeline {
    agent any
    // Replace with your Docker Hub image name
    environment {
        DOCKER_IMAGE_NAME = "vinod812/train-schedule"
    }
    stages {
        stage("Checkout from GitHub Repo") {
            steps {
                git url: 'https://github.com/vinod812/my-nginx.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                echo 'Running build automation'
                // Use full path if gradle is not in PATH
                bat 'C:\\Gradle\\gradle-8.5\\bin\\gradle.bat build --no-daemon'
                archiveArtifacts artifacts: 'build/libs/*.jar'
            }
        }
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
