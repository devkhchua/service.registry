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
                sh '''
                    [ -d ~/.ssh ] || mkdir ~/.ssh && chmod 0700 ~/.ssh
                    ssh-keyscan -t rsa,dsa 192.168.0.100 >> ~/.ssh/known_hosts
                    ssh kube_machine ...
                '''
                }
            }
        }
    }
}