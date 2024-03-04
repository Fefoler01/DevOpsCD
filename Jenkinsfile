node {

    def dockerHub = 'vincem01' // use your docker hub username!

    def ip = 'host.docker.internal'
    def mvnHome = tool 'Maven'
    def dockerImage = 'devopscd' // this is also the name of the docker container
    def dockerImageTag = "${env.BUILD_NUMBER}"


    /*stage('Hello'){
        echo 'Hello World'
    }*/

    stage('Git Pull'){
        //sh 'ls -A1 | xargs rm -rf'
        sh 'ls -la'
        sh 'git clone https://github.com/Fefoler01/DevOpsCD.git . || git pull'
        sh 'ls -la'
    }
    
    stage('Build Project'){
        sh "'${mvnHome}/bin/mvn' -B -DskipTests clean package"
    }

    stage('Initialize Docker'){
        def dockerHome = tool 'Docker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
        sh 'docker -H tcp://${ip}:2375 ps'
    }

    /*stage('Clean Docker'){
        sh "docker -H tcp://${ip}:2375 stop ${dockerImage} || true"
        sh "docker -H tcp://${ip}:2375 rm ${dockerImage} || true"
        sh "docker -H tcp://${ip}:2375 rmi -f \$(docker -H tcp://${ip}:2375 images -q ${dockerImage}) || true"
    }*/
    
    /*stage('Build Docker Image'){
        sh "docker -H tcp://${ip}:2375 build -t ${dockerImage}:${dockerImageTag} ."
    }*/

    /*stage('Deploy'){
        sh "docker run --name ${dockerImage} -d -p 2222:2222 ${dockerImage}:${dockerImageTag}"
    }*/

    /*stage('Publish'){
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_HUB_USR', passwordVariable: 'DOCKER_HUB_PSW')]) {
            sh "docker -H tcp://${ip}:2375 login -u $DOCKER_HUB_USR -p $DOCKER_HUB_PSW"
            sh "docker -H tcp://${ip}:2375 image tag ${dockerImage}:${dockerImageTag} ${dockerHub}/${dockerImage}:${dockerImageTag}"
            sh "docker -H tcp://${ip}:2375 push ${dockerHub}/${dockerImage}:${dockerImageTag}"
        }
    }*/

    stage('Create deployment and service file'){
        writeFile file: 'deployment.yaml',
        text: """
        apiVersion: apps/v1
        kind: Deployment
        metadata:
            name: ${dockerImage}
            namespace: dev
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
            namespace: dev
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

    stage('Install minikube'){
        sh 'apt-get install -y sudo'
        // Télécharger et installer Minikube
        sh 'curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/ && minikube version'

        // Télécharger et installer kubectl
        sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl && kubectl version --client'
    }

    stage('Démarrage de Minikube') {
        // Démarrer Minikube avec la configuration souhaitée
        sh 'minikube start --kubernetes-version=v1.23.0 --memory=4096 --cpus=2 --docker-env DOCKER_HOST=tcp://${ip}:2375'
    }

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