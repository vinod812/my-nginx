pipeline {
    agent any
    // Replace with your Docker Hub image name
    environment {
        DOCKER_IMAGE_NAME = "vinod812/train-schedule"
    }
    stages {
         stage("Checkout from GitHub Repo") {
   steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vinod812/my-nginx.git']])
            }
         }

    stage('Build Docker Image') {
      steps {
        bat 'docker build -t my-app-image .'
      }
    }
        
    /*    stage('Build') {
            steps {
                echo 'Running build automation'
                // Use full path if gradle is not in PATH
                bat 'D:\\tool\\gradle-8.13-all\\gradle-8.13\\bin\\gradle.bat build --no-daemon'
                archiveArtifacts artifacts: 'app/build/libs/*.jar'
            }
        }*/
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
