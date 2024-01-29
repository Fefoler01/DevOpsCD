pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    def version = env.BUILD_ID
                    sh "mvn clean package -Drevision=${version}"
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = "your-docker-username/st2dce:${BUILD_ID}"
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        sh "docker build -t ${dockerImage} ."
                        sh "docker push ${dockerImage}"
                    }
                }
            }
        }
    }
}
