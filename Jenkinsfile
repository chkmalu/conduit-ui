pipeline {
    agent any

    environment {
        IMAGE_REPO = "https://registry-1.docker.io/"
        IMAGE_NAME = "conduit-ui"
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('BUILD & PUSH IMAGE') {
            steps {
                echo 'BUILDING & PUSHING IMAGE'
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_ACCESS', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        sh "echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin ${IMAGE_REPO}"
                        sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                        sh "docker tag seamlesshr:1.0 ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}"
                        sh "docker push ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }
        stage('DEPLOY CONDUIT-UI') {
            environment {
                DOCKER_HUB = credentials('DOCKER_HUB_ACCESS')
            }
            steps {
                echo 'DEPLOYING CONDUIT-UI'
                
                dir('kubernetes') {
                    sh 'aws eks update-kubeconfig --region us-east-1 --name conduit_cluster'
                    sh "kubectl create secret docker-registry regcred --docker-server=${IMAGE_REPO} \
                        --docker-username=${DOCKER_HUB_USR} \
                        --docker-password=${DOCKER_HUB_PSW}"
                    sh 'kubectl apply -f conduit-ui.yaml'
                }
            }
        }
    }
}