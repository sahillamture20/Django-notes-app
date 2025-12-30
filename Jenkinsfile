pipeline {
    agent { label "sahil" }
    
    stages {
        stage("Code"){
            steps {
                echo "This is cloning the code..."
                git url: "https://github.com/sahillamture20/Django-Notes-app.git", branch: "main"
                echo "Code cloning successfull..."
            }
        }
        stage("Build"){
            steps {
                echo "This is building the code..."
                sh "docker build -t notes-app ."
            }
        }
        stage("Push Image to Docker Hub"){
            steps {
                echo "Pushing image to DOcker Hub..."
                withCredentials([usernamePassword(
                    'credentialsId':'DockerHubCred',
                    passwordVariable:'DockerHubPass',
                    usernameVariable:'DockerHubUser'
                    )]){
                        sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                        sh "docker image tag notes-app:latest ${env.DockerHubUser}/notes-app:latest"
                        sh "docker push ${env.DockerHubUser}/notes-app:latest "
                        
                    }
            }
        }
        stage("Deploy"){
            steps {
                echo "This is deploying the code..."
                sh "docker run -d -p 8000:8000 notes-app:latest"
                echo "Code deployment successful..."
            }
        }
    }
}
