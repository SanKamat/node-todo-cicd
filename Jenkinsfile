pipeline{
    agent any
  stages{
    stage("code"){
         steps{
             git url:'https://github.com/SanKamat/node-todo-cicd.git',branch:'master'
         }
    }
    stage("Build & test"){
         steps{
             sh "docker build . -t node-app-demo"
         }
    }
    stage("Push to repository"){
         steps{
             withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubpass",usernameVariable:"dockerHubuser")]){
                 sh "docker login -u ${env.dockerHubuser} -p ${env.dockerHubpass}"
                 sh "docker tag node-app-demo ${env.dockerHubuser}/node-app-demo:latest"
                 sh "docker push ${env.dockerHubuser}/node-app-demo:latest"
             }
         }
    }
    stage("Deploy"){
        steps{
            sh "docker-compose down && docker-compose up -d"
        }
    }
  }
}
