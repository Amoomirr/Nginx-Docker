pipeline {
    agent { label "dev" }  // Use the agent labeled 'dev'

    stages {

        stage("Code") {
            steps {
                // Clone the repository from GitHub
                git url: "https://github.com/Amoomirr/Nginx-Docker.git", branch: "master"
                echo "Code cloned from GitHub"
            }
        }

        stage("Trivy Scan") {
            steps {
               script 
                { // Scan the local filesystem for vulnerabilities using Trivy
                sh "trivy fs . -o trivy-results.json"
                echo "Trivy scan completed"
                }
            }
        }

        stage("Build") {
            steps {
                script {
                    // Build Docker image from Dockerfile
                    sh "docker build -t my-nginx-app ."
                    echo "Docker image built successfully"
                }
            }
        }

        stage("Test") {
            steps {
                script {
                    // Placeholder for test scripts to be implemented by developers
                    echo "Running test scripts (to be implemented)"
                }
            }
        }

        stage("Push") {
            steps {
                script {
                    // Authenticate and push Docker image to DockerHub
                    withCredentials([usernamePassword(
                        credentialsId: "dockerhubid",
                        usernameVariable: "dockeruser",
                        passwordVariable: "dockerpasswd"
                    )]) {
                        sh "docker login -u ${dockeruser} -p ${dockerpasswd}"
                        sh "docker image tag my-nginx-app ${dockeruser}/my-nginx-app:latest"
                        sh "docker push ${dockeruser}/my-nginx-app:latest"
                        echo "Docker image pushed to DockerHub"
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    // Deploy the application using Docker Compose
                    sh "docker compose up --build -d"
                    echo "Application deployed successfully"
                }
            }
        }

    }
}
