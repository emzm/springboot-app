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
        
        stage('Image push to local Docker registry') {
            steps {
                script {
                    def dockerImage = 'springbootjacoco:0.0.1'  // Update with your image tag and name
                    
                    // Build Docker image
                    sh "docker build -t dockerregistry.com/${dockerImage} -f Dockerfile ."
                    
                    // Authenticate and push to Docker registry
                    withDockerRegistry(credentialsId: 'docker-cred', url: 'https://dockerregistry.com/v2/') {
                        sh "docker push dockerregistry.com/${dockerImage}"
                    }
                }
            }
        }
    }
}
