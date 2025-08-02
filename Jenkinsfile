pipeline {
    agent {label "dev"} ; 

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/Amoomirr/Nginx-Docker.git", branch: "master"  #clones url from the github to my agent
                echo "Code is cloned"
            }
        }
        stages {
            stage("Trivy Scan")
            {
                steps {
                    sh "trivy fs . -o results json" #it scans and save any vulnerabilities found
                }
            }
        }

        stage("Build") {
            steps {
                script {
                    sh "docker build -t my-nginx-app ." # builds the image from dockerfile
                    echo "Code is build"
                }
            }
        }
        stage("Test"){
            steps{
                script{
                    echo "Testing" #testcode will be given by developer
                }
            }
        }
        stage("Push"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: "dockerhubid", #uses dockerhub credentials which we created and saved_in_jenkins
                    usernameVariable: "dockeruser", #passed the value of credentials to the variable 
                    passwordVariable: "dockerpasswd")]){
                        sh "docker login -u ${env.dockeruser} -p ${env.dockerpasswd}" #login to our dockerhub account
                        sh "docker image tag my-nginx-app ${env.dockeruser}/my-nginx-app" #creates a tag to our image from_build_stage
                        sh "docker push ${env.dockeruser}/my-nginx-app:latest" #push the image to dockerhub
                    echo "Code is Pushed to DockerHub"
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    sh "docker compose up --build -d" #Used to build container and was specially used for_port mapping which can also be done by dockerfile but_as it is CI/CD we dont want port already exist error.
                    echo "Code is Deployed and working"
                }
            }
        }
    }
}
