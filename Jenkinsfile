pipeline {
    agent any
    
    environment {
        dockerImage = 'springbootjacoco:0.0.1'  // Define your Docker image name and tag
    }
    
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
        
        stage('Image push to local Docker registry') {
            steps {
                script {
                    // Docker Build
                    docker.build("${dockerImage}", "-f Dockerfile .")
                    
                    // Docker Push
                    docker.withRegistry('https://dockerregistry.com', 'docker-cred') {
                        docker.image("${dockerImage}").push()
                    }
                }
            }
        }
    }
}
