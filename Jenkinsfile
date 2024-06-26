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
                    docker.withRegistry('https://hub.docker.com/u/1zee', 'docker-cred') {
                        def customImage = docker.image('1zee/springboot-app:tagname')
                        customImage.push()
                    }
                }
            }
        }
    }
}
