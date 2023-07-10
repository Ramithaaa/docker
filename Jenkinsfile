pipeline {
    agent any
    stages {
        stage("code") {
            steps {
                git branch: "main",url: "https://github.com/LondheShubham153/django-notes-app.git"
            }
        }
        stage("Build") {
            steps {
                sh "docker build -t notes ."
            }
        }
        stage("Push") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",usernameVariable:"dockerHubUser",passwordVariable:"dockerHubPass")]) {
                    sh "docker tag notes ${env.dockerHubUser}/notes:v1"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/notes:v1"
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
