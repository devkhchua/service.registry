pipeline {
    environment {
        imagename = "devkhchua/service.registry"
        registryCredential = 'docker_credentials'
        dockerImage = ''
     }

    agent any
    stages {
        stage('Initialize') {
            steps{
                script{
                    def dockerHome = tool 'Docker'
                    def mavenHome  = tool 'Maven3'
                    env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
                }
            }
        }

        stage('Building Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Running Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Building Docker Image') {
            steps{
                script {
                  dockerImage = docker.build imagename
                }
            }
        }

        stage('Deploying Docker Image') {
            steps{
                script {
                  docker.withRegistry( '', registryCredential ) {
                    dockerImage.push('latest')
                  }
                }
            }
        }

        stage('Removing Unused Docker Image') {
            steps{
                sh "docker rmi $imagename:latest"
            }
        }

        stage ('Deploy into Kubernetes') {
            steps{
                sshagent(credentials : ['KUBE_MACHINE']) {
                    sh 'ssh -v 192.168.0.100'
                    sh 'ssh 192.168.0.100 kubectl apply -f C:/Coding/projects/learning-java/k8s/service-registry.yml'
                    sh 'ssh 192.168.0.100 kubectl delete pod eureka-0'
                    sh 'ssh 192.168.0.100 kubectl get pods'
                }
            }
        }
    }
}