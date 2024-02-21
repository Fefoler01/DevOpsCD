node {

    def mvnHome = tool 'Maven'
    def dockerImage
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"


    stage('Hello') {
        echo 'Hello World'
    }

    // clone the repository from GitHub
    stage('Clone') {
        git 'https://github.com/Fefoler01/DevOpsCD.git'
    }
    
    stage('Build') {
        script {
            def version = env.BUILD_ID
            sh "${tool 'Maven'}/bin/mvn clean package -Drevision=${version}"
        }
    }
    
    stage('Build Docker Image') {
        sh "docker build -t ${dockerImage} ."
        sh "docker push ${dockerImage}"
    }
}