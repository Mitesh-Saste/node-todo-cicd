 pipeline{
     agent { label 'dev'}
     stages {
         stage("Clone Code"){
             steps{
                 echo "Cloning the code"
                 git url: "https://github.com/iam-harshsoni/node-todo-cicd", branch: "master"
             }
         }
         stage("Build"){
               steps{
                   echo "Building the docker image"
                   sh "docker build -t my-todo-app ."
             }
         }
         stage("Push to DockerHub"){
               steps{
                   echo "Pushing the image to DockerHub"
                   withCredentials([usernamePassword(credentialsId:"dockerHubCred",
                   passwordVariable:"dockerHubPass",
                   usernameVariable:"dockerHubUser")]){
                   sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                   sh "docker image tag my-todo-app:latest ${env.dockerHubUser}/my-todo-app:latest"
                   sh "docker push ${env.dockerHubUser}/my-todo-app:latest"
                   }
             }
         }
         stage("Deploy"){
               steps{
                   echo "Deploying the container"
                   sh "docker compose down"
                   sh "docker compose up -d" 
             }
         }
     }
 }
