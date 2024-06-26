pipeline {
    agent {
        docker {
            image 'maven:3.8.1-jdk-11'
            label 'docker-agent'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Docker Build and Push') {
            environment {
                DOCKER_REGISTRY = "hub.docker.com"
                DOCKER_CREDENTIALS_ID = "docker-cred"
            }
            steps {
                script {
                    docker.withRegistry('https://${DOCKER_REGISTRY}', DOCKER_CREDENTIALS_ID) {
                        def app = docker.build("1zee/my-app:${env.BUILD_NUMBER}")
                        app.push()
                    }
                }
            }
        }
    }
}
