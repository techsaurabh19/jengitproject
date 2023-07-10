pipeline {
    agent any

    stages {
        stage('CLone Code') {
            steps {
                echo "Cloning the code"
                git url:"https://github.com/techsaurabh19/jengitproject.git",branch: "main"
            }
            
        }
        stage('build'){
            steps{
                echo "Build the image"
                sh "docker build -t my-node-app ."
                
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing the imaeg to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-node-app ${env.dockerHubUser}/my-node-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-node-app:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                echo " Deploying the container"
                sh "docker-compose down && docker-compose up -d "
            }
        }
    }
}
