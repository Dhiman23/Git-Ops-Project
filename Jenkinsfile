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
        stage('Push Docker image'){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker images'){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }

     stage('updating kubernetes manifest file'){
        steps{
            script{
                sh """
                 cat deployment.yml
                 sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                 cat deployment.yml
                """
            }
        }
     }
    }
}