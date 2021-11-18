pipeline {
    agent any

    stages {
        stage('Checkout source') {
            steps {
                git url: 'https://github.com/selton00/flask-api.git', branch: 'develop'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("seltonlucas/flask-api:v${env.BUILD_ID}",
                        '-f ./Dockerfile .')
                }
            }
        }

        stage('Push Image') {
            steps {
                script  {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}