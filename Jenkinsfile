pipeline {
    agent any

    stages {
        stage('Build Stage') {
            steps {
                echo 'Building'
                withCredentials([usernamePassword(credentialsId: 'DockerHubAccess', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh "echo ${password} | docker login -u ${USERNAME} --password-stdin"
                    sh "docker build -t conduit-ur:latest ."
                    sh "docker tag conduit-ui:latest chkmalu/conduit-ui:latest"
                    sh "docker push chkmalu/conduit-ui:latest"
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