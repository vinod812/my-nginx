pipeline {
    agent any
    environment {
        //be sure to replace "DOCKER_IMAGE_NAME" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "vinod812/train-schedule"
    }
    stages {
        stage("Checkout from github repo"){
            steps{
            # Replace with your github repo
            git url: 'https://github.com/vinod812/my-nginx.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
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
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            steps {
                kubeconfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeU1USXdOREUwTXpZME5sb1hEVE15TVRJd01URTBNelkwTmxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTVV4CnR2eFdmcmNGV2crVFpxSi92U3pWRFlPSVVMWE0zMlRxOW9TL1g5UzZVVjNkZ0xUTDI4V0IvblF0K0xGbjNwWEQKOE9RNTZVN25sZlhiSWI0Q1ZUdVVkVDg0c2xGcVdWN2lkUVlGNXVNaERDamF2MW16OG9IbVJQVXlBVW9HQWY5VApWWUFxY3h2VnVvbDVKa1EydXpyTHJrZ1VJU2k3dVdYdlVxRnl4bW8rUWt2cWRiUm5ldDBpbHNTbU5FdDlObTNTClVJSStrbTRZU3lod2xtTHlOYXJPc0lsNVU3ZXpuUG1Ua0E0MXN1ZG4wSjNRMXBYQUdSY2NqVXVzWjBIK3FyZm4KOGJWdFpuMmhsbms0VHRMSGhEUS81MFNXOCt1MEVLOHo2Sms5bHVET1BiSjdxam11U0Y1c3RJUDg1VWo2SndhMgpoUm1sclU1UUlzSTd5ZWlFMzZFQ0F3RUFBYU5aTUZjd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZQcFhkVmc3LzJiUGhBZytZdys4WFIzWk90OUJNQlVHQTFVZEVRUU8KTUF5Q0NtdDFZbVZ5Ym1WMFpYTXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBR2IwcEEwQythVFVDdytOY1FyZgp3NHNhQStUMnNMU0xTM3NGOWh1NEdKU0lHaXdPTDRja2pscE1sUTE5aDltWnVpdVBkOEhNa0RrWTBTRHQvRXdyCm94MVdrYTlxMkFEZ0hyVCtDUzBOWHVJMDBLQ0QzdDd5czBhdGl6b21lWnppcE9aYkZRdnI2eEU0bUhQY0hNQUEKQ1dMR2RvRjNhVEZFODJIUWxLWkM4anFHUklqeE5SWlFpZ1IvVXlxSktYZWs1Z1BQZGVzckJRY2NuQzVtVWxoOQo0cktwT1p0UTBjajFMakx2dkQxNWdCMkVSVzBtK2lDTUhCM1g1SGJXdkZWOEgzOEFzcjc5amwrMHFoU1lRTTVoCmsweVNxd3gvY2hFd0hWc0NURjNKSUZUejAwZVhFa0VPdE1yVitYWEhKNk9jdzFINXFvNFhHY0NoUDN2c3NZbFMKUWw0PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==', credentialsId: 'kubernetes', serverUrl: 'https://192.168.59.104:8443') {
    // some block
                 sh 'kubectl apply -f deployment.yaml'
                 sh 'kubectl apply -f app-service.yaml'
                 sh 'kubectl rollout restart deployment train-schedule'
}
            }
        }
    }
}
