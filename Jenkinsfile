pipeline {
    agent any
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url: "https://github.com/hemelsre/django-notes-app.git", branch: "main"
            }
        }
        
        stage("Build"){
            steps {
                echo "Building the code"
                sh "docker build -t my-note-app ."
            }
        }
        
        stage("Push to dockerhub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker tag my-note-app ${env.dockerhubuser}/my-note-app:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/my-note-app:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
