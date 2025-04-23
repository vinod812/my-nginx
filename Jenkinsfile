pipeline {
    agent any
    environment {
        //be sure to replace "DOCKER_IMAGE_NAME" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "vinod812/train-schedule"
    }
    stages {
        stage("Checkout from github repo"){
            steps{
            git url: 'https://github.com/vinod812/my-nginx.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
  

    }
}
