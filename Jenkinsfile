pipeline{
    agent any

    environment{
        DOCKERHUB_USERNAME = "sajaldhimanitc1999"
        APP_NAME = "git-ops-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}"+"/"+"${APP_NAME}"
        REGISTRY_CREDS = 'docker'

    }

    stages{
        stage('CleanUp workspace'){
            steps{
                script{
                    cleanWs()
                }
            }

        }
        stage('checkOut SCM'){
            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/Dhiman23/Git-Ops-Project.git',
                    branch: 'main'
                }
            }
        }

        stage('Build docker Image'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"

                }
            }
        }
    }
}