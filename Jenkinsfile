pipeline {
    agent any

    stages {
        stage('Build Stage') {
            steps {
                echo 'Building'
                withCredentials([usernamePassword(credentialsId: 'DockerHubAccess', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh "echo ${password} | docker login -u ${USERNAME} --password-stdin"
                    sh "docker build -t conduit-ui:latest ."
                    sh "docker tag conduit-ui:latest chikamalu/conduit-ui:latest"
                    sh "docker push chikamalu/conduit-ui:latest"
                }

            }
        }
        stage('Testing Stage') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deploy Stage') {
            steps {
                echo 'Deploying'
            }
        }
    }
}