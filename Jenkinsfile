pipeline {
    agent any
    stages{
        stage("Clone the Code from GITHub") {
            steps{
                git url: 'https://github.com/sujitsahoo0285/node-todo-cicd.git', branch: 'master'
            }
        }
        stage("Build & Test") {
            steps{
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("Push to DockerHub") {
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app-demo ${env.dockerHubUser}/node-app-test-new:latest"
                    sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                }
            }
        }
        stage("Deploy image on the EC2") {
            steps{
                sh "docker-compose down"
                sh "docker-compose up -d"
            }
        }

    }
}
