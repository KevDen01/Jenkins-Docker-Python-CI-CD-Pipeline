pipeline {
    agent any

    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/KevDen01/jenkins_pipeline.git", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building the Docker image"
                sh "sudo docker build -t pipeline ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "sudo docker tag pipeline ${env.dockerHubUser}/pipeline:latest"
                    sh "sudo docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "sudo docker push ${env.dockerHubUser}/pipeline:latest"
                }
            }
        }

        stage ("Deploy") {
            steps {
                echo "Deploying the container"
                sh "sudo docker compose down && docker compose up -d"
            }
        }
    }
}
