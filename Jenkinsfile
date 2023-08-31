pipeline {
    agent any

    stages {
        stage ('Checking out GitHub Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/RicardoTerraform/pipeline-react.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    def dockerapp = docker.build("ricardoterraform/clientes:${env.BUILD_ID}", '-f ./client/Dockerfile ./client')
                }
            }
        }

        stage ('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com/', 'dockerhub'){
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}
