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
                    sh 'ssh -o PreferredAuthentications=privatekey Jordan@192.168.0.100 uptime'
                    sh 'ssh -v Jordan@192.168.0.100'
                }
            }
        }
    }
}