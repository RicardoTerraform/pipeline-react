def COMMIT_ID = ''
pipeline {
    agent any

    stages {
        stage ('Checking out GitHub Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/RicardoTerraform/pipeline-react.git'
                
                script {
                    //windows bat-linux sh
                    COMMIT_ID = sh (
                        script: "git log -n 1 --pretty=format:'%H'",
                        returnStdout: true)
                    //COMMIT_ID = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                    echo "**************************************************"
                    echo "ricardoterraform/clientes:${COMMIT_ID}"
                    echo "**************************************************"
                }
                
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("ricardoterraform/clientes:${COMMIT_ID}", '-f ./client/Dockerfile ./client')
                    echo "Image built successfully"
                }
            }
        }

        stage ('Push Image to DockerHub') {
            steps {
                script {
                    echo "pushing image to docker hub"
                    docker.withRegistry('https://registry.hub.docker.com/', 'dockerhub'){
                        dockerapp.push("${COMMIT_ID}")
                    }
                    echo "image pushed successfully to docker container registry"
                }
            }
        }

        stage ('Update Image FrontEnd') {
            steps {
                //git branch: 'main', url: 'https://github.com/RicardoTerraform/pipeline-react.git'
                git(
                    url: "https://github.com/RicardoTerraform/pipeline-react.git",
                    branch: "main",
                    changelog: true,
                    poll: true
                )

                script{
                    def text = readFile "infra/01-client-deploy.yaml"
                    text = text.replaceAll("image:.*", "image: ricardoterraform/client:${COMMIT_ID}")
                    writeFile file: "infra/01-client-deploy.yaml", text: text
                    //sh("cat infra/01-client-deploy.yaml")
                }

                sh 'git add infra/01-client-deploy.yaml'
                sh "git commit -am 'image tag updated by jenkins'"
                sh "git push -f origin main"
            }
        }

    }
}
