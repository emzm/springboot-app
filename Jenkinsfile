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
        stage('Publishing Junit Tests report') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }   
        }
        stage('Publishing Code Coverage') {
            steps {
                jacoco()
            }   
        }
        stage('Docker Image Push') {
            steps {
                script {
                    def commitSHA = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    def dockerTag = "1zee/springboot-app:${commitSHA}"
                    
                    docker.withRegistry('https://hub.docker.com', 'docker-cred') {
                        def customImage = docker.image(dockerTag)
                        customImage.push()
                    }
                }
            }
        }
    }
}
