pipeline{
    
    agent{
        node{
            label "dev"
        }
    }
    
    stages{
        stage("Code Clone"){
            steps{
                echo "Code clone is done here"
                git url: "https://github.com/RahulAggarwal-DevOps/django-notes-app.git", branch: "main"
            }
        }
        stage("Build and Test"){
            steps{
                echo "Build and test is done here"
                sh "whoami"
                sh "docker build -t django-notes-app-jenkins:v1 ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                echo "Push to DockerHub is done here"
                withCredentials(
                    [
                        usernamePassword(
                            credentialsId: "DockerHubCreds",
                            usernameVariable: "DockerHubUser",
                            passwordVariable: "DockerHubPasswd"
                        )
                    ]
                ){
                sh "docker image tag django-notes-app-jenkins:v1 ${env.DockerHubUser}/django-notes-app-jenkins:v1"
                sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPasswd}"
                sh "docker push ${env.DockerHubUser}/django-notes-app-jenkins:v1"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Application Deployment"
                sh "docker compose up -d"
            }
        }
    }
}
