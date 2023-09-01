def COMMIT_ID = ''
pipeline {
    agent any

    stages {
        stage ('Checking out GitHub Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/RicardoTerraform/pipeline-react.git'
                
                script {
                    // // Determine the shell command based on the OS
                    // def gitCommand
                    // if (isUnix()) {
                    //     gitCommand = "git log -n 1 --pretty=format:'%H'"
                    // } else {
                    //     gitCommand = "git log -n 1 --pretty=format:\"%H\""
                    // }

                    // COMMIT_ID = sh(script: gitCommand, returnStdout: true).trim()
                    // echo "Commit ID is: ${COMMIT_ID}"



                    //windows bat-linux sh
                    COMMIT_ID = sh (
                        script: "git log -n 1 --pretty=format:'%H'",
                        returnStdout: true).trim()
                    
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
