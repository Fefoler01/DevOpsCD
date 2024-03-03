node {

    def dockerHub = 'vincem01' // use your docker hub username!

    def ip = 'host.docker.internal'
    def mvnHome = tool 'maven'
    def dockerImage = 'devopscd' // this is also the name of the docker container
    def dockerImageTag = "${env.BUILD_NUMBER}"


    /*stage('Hello'){
        echo 'Hello World'
    }*/

    stage('Git Pull'){
        sh 'git clone https://github.com/Fefoler01/DevOpsCD.git'
        sh 'cd DevOpsCD'
        sh 'ls -la'
        sh 'git checkout main'
        sh 'git pull'
    }
    
    stage('Build Project'){
        sh "'${mvnHome}/bin/mvn' -B -DskipTests clean package"
    }

    stage('Initialize Docker'){
        def dockerHome = tool 'docker'
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

    /*stage('Deploy'){
        sh "docker -H tcp://${ip}:2375 run --name ${dockerImage} -d -p 2222:2222 ${dockerImage}:${dockerImageTag}"
    }*/

    stage('Publish'){
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_HUB_USR', passwordVariable: 'DOCKER_HUB_PSW')]) {
            sh "docker -H tcp://${ip}:2375 login -u $DOCKER_HUB_USR -p $DOCKER_HUB_PSW"
            sh "docker -H tcp://${ip}:2375 image tag ${dockerImage}:${dockerImageTag} ${dockerHub}/${dockerImage}:${dockerImageTag}"
            sh "docker -H tcp://${ip}:2375 push ${dockerHub}/${dockerImage}:${dockerImageTag}"
        }
    }

    stage('Create deployment and service file'){
        writeFile file: 'deployment.yaml',
        text: """
        apiVersion: apps/v1
        kind: Deployment
        metadata:
            name: ${dockerImage}
        spec:
            replicas: 2
            selector:
                matchLabels:
                    app: ${dockerImage}
            template:
                metadata:
                    labels:
                        app: ${dockerImage}
                spec:
                    containers:
                    - name: ${dockerImage}
                    image: ${dockerHub}/${dockerImage}:${dockerImageTag}
                    ports:
                    - containerPort: 2222
        ---
        apiVersion: v1
        kind: Service
        metadata:
            name: ${dockerImage}
        spec:
            selector:
                app: ${dockerImage}
            ports:
            - protocol: TCP
            port: 80
            targetPort: 2222
            type: LoadBalancer
        """
    }

    /*stage('Install minikube'){
        sh 'minikube start'
    }*/

    stage('Deploy to Kubernetes'){
        sh 'kubectl get pods'
        //sh 'kubectl apply -f deployment.yaml'
        /*kubernetesDeploy(
            configs: 'deployment.yaml'
        )*/
    }

    /*stage('Deploy to Kubernetes'){
        withCredentials([usernamePassword(credentialsId: 'k8s-credentials', usernameVariable: 'K8S_USR', passwordVariable: 'K8S_PSW')]) {
            sh "kubectl config set-credentials k8s-user --username=$K8S_USR --password=$K8S_PSW"
            sh "kubectl config set-context k8s --cluster=minikube --user=k8s-user"
            sh "kubectl config use-context k8s"
            sh "kubectl set image deployment/${dockerImage} ${dockerImage}=${dockerHub}/${dockerImage}:${dockerImageTag}"
        }
    }*/
}