node {

    def mvnHome = tool 'Maven'
    def dockerImage
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"


    stage('Hello') {
        steps {
            echo 'Hello World'
        }
    }

    // clone the repository from GitHub
    stage('Clone') {
        steps {
            git 'https://github.com/Fefoler01/DevOpsCD.git'
        }
    }
    
    stage('Build') {
        steps {
            script {
                def version = env.BUILD_ID
                sh "${tool 'Maven'}/bin/mvn clean package -Drevision=${version}"
            }
        }
    }
    
    stage('Build Docker Image') {
        steps {
            script {
                sh "docker build -t ${dockerImage} ."
                sh "docker push ${dockerImage}"
            }
        }
    }
    
}