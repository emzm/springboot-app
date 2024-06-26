
pipeline {
    agent {
        docker {
            image 'abhishekf5/maven-abhishek-docker-agent:v1'
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemon
        }
    }

    stages {
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Publishing JUnit Tests Report') {
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
                DOCKER_IMAGE = "1zee/ultimate-cicd:${BUILD_NUMBER}"
                REGISTRY_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} ."
                    
                    // Push Docker image
                    docker.withRegistry('https://hub.docker.com', REGISTRY_CREDENTIALS) {
                        def dockerImage = docker.image("${DOCKER_IMAGE}")
                        dockerImage.push()
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
