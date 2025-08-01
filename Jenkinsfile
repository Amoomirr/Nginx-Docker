pipeline {
    agent {lable "dev"} ;

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/Amoomirr/Nginx-Docker.git", branch: "master"
                echo "Code is clone"
            }
        }

        stage("Build") {
            steps {
                script {
                    sh "docker build -t my-nginx-app ."
                    echo "Code is build"
                }
            }
        }
        stage("Test"){
            steps{
                script{
                    echo "Testing code developer dega"
                }
            }
        }
        stage("Push"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: "dockerhubid", 
                    usernameVariable: "dockeruser",
                    passwordVariable: "dockerpasswd")]){
                        sh "docker login -u ${env.dockeruser} -p ${env.dockerpasswd}"
                        sh "docker image tag my-nginx-app ${env.dockeruser}/my-nginx-app"
                        sh "docker push ${env.dockeruser}/my-nginx-app:latest"
                    echo "Code is Pushed to DockerHub"
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    sh "docker compose up --build -d"
                    echo "Code is Deployed and working"
                }
            }
        }
    }
}
