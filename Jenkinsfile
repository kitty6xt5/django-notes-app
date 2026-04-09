@Library('shared') _
pipeline {
    agent { label "01" }

    environment {
        DOCKER_IMAGE = "kitty6xt5/notes-app:latest"
    }

    stages {

        stage("hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("code"){
            steps{
                script{
                clone("https://github.com/LondheShubham153/django-notes-app","main")
                }
            }
        }

        stage("build"){
            steps{
                echo "Building Docker image..."
                sh "docker build -t notes-app:latest ."
            }
        }

        stage("Push to DockerHub"){
            steps{
                echo "Bhai i am Pushing image into DockerHub..."

                withCredentials([usernamePassword(
                    credentialsId: "dockerhubcredRW",
                    usernameVariable: "DOCKER_USER",
                    passwordVariable: "DOCKER_PASS"
                )]) {

                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker tag notes-app:latest $DOCKER_USER/notes-app:latest
                    docker push $DOCKER_USER/notes-app:latest
                    '''
                }
            }
        }

        stage("deploy"){
            steps{
                echo "Deploying container..."

                sh '''
                docker rm -f notes-container || true
                docker run -d -p 8000:8000 --name notes-container kitty6xt5/notes-app:latest
                '''
            }
        }
    }
}
