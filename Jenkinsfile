pipeline {
    agent any
    stages{
        stage("Clone the Code from GITHub") {
            steps{
                git url: 'https://github.com/sujitsahoo0285/node-todo-cicd', branch: 'master'
            }
        }
        stage("Build & Test") {
            steps{
                sh "docker build . -t node-todo-cicd"
            }
        }
        stage("Push to DockerHub") {
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-todo-cicd ${env.dockerHubUser}/node-todo-cicd:latest"
                    sh "docker push ${env.dockerHubUser}/node-todo-cicd:latest"
                }
            }
        }
        stage("Deploy image on the EC2") {
            steps{
                sh "docker-compose down --remove-orphans"
                sh "docker-compose up -d"
            }
        }

    }
}
