pipeline {
    agent any
    stages {
        stage ('SCM') {
            steps {
                git branch: 'main',
                url: 'https://github.com/shashikamle99/project-01.git'
            }
        }
        stage ('Build the Docker image') {
            steps {
                sh "docker build -t notes-app ."
            }
        }
        stage ('Image tag change') {
            steps {
                echo 'Tag docker images'
                // sh "docker tag notes-app shashikamle99/notes-app:latest"
                sh "docker tag notes-app shashikamle99/notes-app:1.${env.BUILD_NUMBER}"
            }
        }
        stage ('Docker login & Push to Docker hub') {
            steps {
                echo 'Pushing image to dockerhub'
                withCredentials([usernamePassword(credentialsId: 'DockerHub_Creds', passwordVariable: 'DockerPass', usernameVariable: 'DockerUser')])  {
                sh "docker login -u ${env.DockerUser} -p ${env.DockerPass}"
                // sh "docker push ${env.DockerUser}/notes-app:latest"
                sh "docker push ${env.DockerUser}/notes-app:1.${env.BUILD_NUMBER}"
                }
            }
        }
        stage ('Remove docker images') {
            steps {
                // sh "docker system prune -f --all"
                sh 'docker rm -f $(docker ps -aq)'
                sh 'docker rmi -f $(docker images -aq)'
                // sh 'sleep 30'
            }
        }
        stage ('Deployement') {
            steps {
                echo 'Deploying container'
                sh "docker container run -d -p 8000:8000 shashikamle99/notes-app:1.${env.BUILD_NUMBER}"
            }
        }
    }
}