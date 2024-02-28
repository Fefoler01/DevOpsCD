node {

    def dockerHub = 'vincem01' // use your docker hub username!

    def ip = 'host.docker.internal'
    def mvnHome = tool 'Maven'
    def dockerImage = 'devopscd' // this is also the name of the docker container
    def dockerImageTag = "${env.BUILD_NUMBER}"


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

    stage('Clean Docker'){
        sh "docker -H tcp://${ip}:2375 stop ${dockerImage} || true"
        sh "docker -H tcp://${ip}:2375 rm ${dockerImage} || true"
        sh "docker -H tcp://${ip}:2375 rmi -f \$(docker -H tcp://${ip}:2375 images -q ${dockerImage}) || true"
    }
    
    stage('Build Docker Image'){
        sh "docker -H tcp://${ip}:2375 build -t ${dockerImage}:${dockerImageTag} ."
    }

    stage('Deploy'){
        sh "docker -H tcp://${ip}:2375 run --name ${dockerImage} -d -p 2222:2222 ${dockerImage}:${dockerImageTag}"
    }

    stage('Publish'){
        environment {
            DOCKER_HUB = credentials('docker-hub-credentials')
        }
        sh "docker -H tcp://${ip}:2375 login -u ${DOCKER_HUB_USR} -p ${DOCKER_HUB_PSW}"
        sh "docker -H tcp://${ip}:2375 image tag ${dockerImage}:${dockerImageTag} ${dockerHub}/${dockerImage}:${dockerImageTag}"
        sh "docker -H tcp://${ip}:2375 push ${dockerHub}/${dockerImage}:${dockerImageTag}"
    }
}