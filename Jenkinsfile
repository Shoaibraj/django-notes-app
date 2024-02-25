pipeline {
    agent any
    
    
    stages {
        stage('code') {
            steps {
                echo 'clonening the code '
                git url:"https://github.com/Shoaibraj/django-notes-app.git", branch: "main"
                }
             }
        stage('build') {
            steps {
                echo 'building the code'
                sh "usermod -aG docker jenkins"
                sh "sudo chmod 666 /var/run/docker.sock"
                sh "docker build -t myapp ."
            }
        }
        stage('push') {
            steps {
                echo 'pushing the code to docker hub'
                withCredentials([usernamePassword(credentialsId:"DockerHub", passwordVariable:"DockerHubPass", usernameVariable:"DockerHubUser")]){
                sh "docker tag myapp ${env.DockerHubUser}/myapp:latest"
                sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                sh "docker push ${env.DockerHubUser}/myapp:latest"
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploying the conatiner'
                sh "apt install docker-compose -y"
                sh "docker-compose down && docker-compose up -d"
            }
        }
   }
}
