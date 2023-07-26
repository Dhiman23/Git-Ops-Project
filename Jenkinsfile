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
        stage('triggering CD pipeline'){
            steps{
                script{
                    sh "curl -v -k -user sajal:1126525ad68690760af49bb3ad6a21dac2 -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' -data 'IMAGE_TAG=${IMAGE_TAG}' 'http://44.217.26.176:8080/job/gitops-argocd-CD/buildWithParameters?token=gitops-config'"
                }
            }
        }
    }
}
