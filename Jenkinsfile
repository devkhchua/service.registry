pipeline {
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
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage("Building Docker Image"){
            steps {
                sh 'docker version'
                sh 'docker build -t devkhchua/service.registry:latest .'
                sh 'docker image list'
            }
        }

        stage("Logging into Docker"){
            steps {
                withCredentials([string(credentialsId: 'DOCKER_PASS', variable: 'PASSWORD')]) {
                    sh 'docker login -u devkhchua -p $PASSWORD'
                }
            }
        }

        stage("Pushing into Docker"){
            steps{
                sh 'docker push devkhchua/service.registry:latest'
            }
        }
    }
}