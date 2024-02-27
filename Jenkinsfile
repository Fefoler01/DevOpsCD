node {

    def ip = 'host.docker.internal'
    def mvnHome = tool 'Maven'
    def dockerImage
    def dockerImageTag = "devopscd:${env.BUILD_NUMBER}"


    stage('Hello'){
        echo 'Hello World'
    }

    stage('Git Pull'){
        sh 'git checkout main'
        sh 'git pull'
    }
    
    stage('Build Project'){
        sh "'${mvnHome}/bin/mvn' -B -DskipTests clean package"
    }

    stage('Initialize Docker'){
        def dockerHome = tool 'Docker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
    
    stage('Build Docker Image'){
        sh "docker -H tcp://${ip}:2375 build -t ${dockerImageTag} ."
    }

    stage('Deploy'){
        sh "doker -H tcp://${ip}:2375 run --name devopscd -d -p 2222:2222 ${dockerImageTag}"
    }
}