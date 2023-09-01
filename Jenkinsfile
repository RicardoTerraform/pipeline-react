def COMMIT_ID = ''
pipeline {
    agent any

    stages {
        stage ('Checking out GitHub Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/RicardoTerraform/pipeline-react.git'
                
                script {
                    //windows bat-linux sh
                    // COMMIT_ID = bat (
                    //     script: "git log -n 1 --pretty=format:'%S'",
                    //     returnStdout: true)
                    COMMIT_ID = bat(returnStdout: true, script: 'git rev-parse HEAD').trim()
                    echo "id is. ${COMMIT_ID}"
                }
                
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("ricardoterraform/clientes:$COMMIT_ID", '-f ./client/Dockerfile ./client')
                    echo "Image built successfully"
                }
            }
        }

        stage ('Push Image') {
            steps {
                script {
                    echo "pushing image to docker hub"
                    docker.withRegistry('https://registry.hub.docker.com/', 'dockerhub'){
                        dockerapp.push("$COMMIT_ID")
                    }
                    echo "image pushed successfully to docker container registry"
                }
            }
        }
    }
}
