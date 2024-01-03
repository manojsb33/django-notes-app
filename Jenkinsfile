pipeline{
    agent any
    stages{
        stage("code"){
            steps {
                echo "clone the code"
                git url: "https://github.com/manojsb33/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps {
                echo "building the code"
                sh "docker build -t my-notes-app ."
            }
        }
        stage("push"){
            steps {
                echo "push to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerpass",usernameVariable:"dockeruser")]){
                    sh "docker tag my-notes-app ${env.dockeruser}/my-notes-app:latest"
                    sh "docker login -u ${env.dockeruser} -p ${dockerpass}"
                    sh "docker push ${env.dockeruser}/my-notes-app:latest"
                }
            }
        }
         stage("deploy"){
            steps {
                echo "deploy to server"
                sh "docker-compose down && docker-compose up -d"
            }
        }         
    }
}

