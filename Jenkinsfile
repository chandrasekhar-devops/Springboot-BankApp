pipeline{
    
    agent { label 'dev' };
    
    stages{
        stage("Code"){
            steps{
                echo "cloning the code ...."
                git url:"https://github.com/chandrasekhar-devops/Springboot-BankApp.git", branch:"cicd"
            }
        }
        stage("Build"){
            steps{
                echo "building code using docker ..."
                sh "whoami"
                sh "docker build -t bankapp:latest ."
            }
        }
        stage("Docker Push"){
            steps{
                echo "Push to docker hub ..."
                withCredentials([usernamePassword(
                    credentialsId:"dockerHub",
                    usernameVariable:"docker_user",
                    passwordVariable:"docker_pass")]){
                        sh 'echo $docker_pass | docker login -u $docker_user --password-stdin'
                        sh "docker image tag bankapp:latest ${env.docker_user}/bankapp:latest"
                        sh "docker push ${env.docker_user}/bankapp:latest"
                    }
                
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploy the code ..."
                sh "docker compose up -d --build mainapp"
            }
        }
    }
    
    
}
