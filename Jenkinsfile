pipeline {
    environment {
        imagename = "devkhchua/service.registry"
        registryCredential = 'docker_credentials'
        dockerImage = ''
     }

    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }

    stages {
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
                    dockerImage.push("$BUILD_NUMBER")
                     dockerImage.push('latest')

                  }
                }
            }
        }

        stage('Removing Unused Docker Image') {
            steps{
                sh "docker rmi $imagename:$BUILD_NUMBER"
                sh "docker rmi $imagename:latest"
            }
        }
    }
}