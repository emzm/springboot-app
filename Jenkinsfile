pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean install'
                sh 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Publishing JUnit Tests report') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }   
        }
        
        stage('Publishing Code Coverage') {
            steps {
                jacoco()
            }   
        }      
        
        stage('Build and Push Docker Image') {
            environment {
                DOCKER_IMAGE = "emzm-cicd-springboot-maven/ultimate-cicd:${BUILD_NUMBER}"
                REGISTRY_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    sh 'cd java-maven-sonar-argocd-helm-k8s/spring-boot-app && docker build -t ${DOCKER_IMAGE} .'
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', REGISTRY_CREDENTIALS) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
