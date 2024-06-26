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

        stage('Docker Image Push') {
            steps {
                script {
                    def commitSHA = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    def dockerTag = "1zee/springboot-app:${commitSHA}"
                    
                    // Login to Docker Hub securely
                    withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Docker login command
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                        
                        // Push Docker image
                        docker.withRegistry('https://registry.hub.docker.com', 'docker-cred') {
                            def customImage = docker.image(dockerTag)
                            customImage.push()
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline successfully completed!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
