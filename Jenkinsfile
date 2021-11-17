pipeline {
    environment {
        imagename = "devkhchua/service.registry"
        registryCredential = 'docker_credentials'
        dockerImage = ''
     }

    agent any
    stages {
        stage ('Deploy into Kubernetes') {
            steps{
                sshagent(credentials : ['KUBE_MACHINE']) {
                    sh 'ssh -v Jordan@192.168.0.100'
                    sh 'ssh Jordan@192.168.0.100 kubectl apply -f C:/Coding/projects/learning-java/k8s/service-registry.yml'
                    sh 'ssh Jordan@192.168.0.100 kubectl get pods -f C:/Coding/projects/learning-java/k8s/service-registry.yml'
                }
            }
        }
    }
}